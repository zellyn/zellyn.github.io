---
layout: page
title: 'Spline editor'
extra_css: |
  .container {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 10px;
  }
  .controls {
    display: flex;
    gap: 20px;
    align-items: center;
    flex-wrap: wrap;
  }
  .point {
    cursor: pointer;
    fill: lightblue;
  }
  .midpoint {
    cursor: pointer;
    fill: pink;
  }
  g.selected .point {
    cursor: pointer;
    fill: blue;
    stroke: blue;
    stroke-width: 3;
  }
  g.selected .midpoint {
    cursor: pointer;
    fill: red;
    stroke: red;
    stroke-width: 1;
  }
  .curve {
    fill: none;
    stroke: black;
    stroke-width: 2;
    cursor: move;
  }
  .curve.selected {
    stroke: #0066cc;
    stroke-width: 3;
  }
  .background-image {
    pointer-events: none;
  }
  label {
    display: flex;
    align-items: center;
    gap: 5px;
  }
  .drawing-controls {
    display: flex;
    gap: 10px;
    align-items: center;
  }
  .drawings-list {
    max-height: 200px;
    overflow-y: auto;
    border: 1px solid #ccc;
    padding: 5px;
  }
  .drawing-item {
    cursor: pointer;
    padding: 2px 5px;
  }
  .drawing-item:hover {
    background: #eee;
  }
  .drawing-item.selected {
    background: #ddd;
  }
---

[< tools](../)

  <div class="container">
    <div class="controls">
      <div class="drawing-controls">
        <input type="text" id="drawingName" placeholder="Drawing name">
        <button id="saveDrawing">Save</button>
        <button id="loadDrawing">Load</button>
        <button id="deleteDrawing">Delete</button>
        <div id="drawingsList" class="drawings-list" style="display: none;"></div>
      </div>
      <input type="file" id="imageInput" accept="image/*">
      <label>
        Image Opacity:
        <input type="range" id="opacitySlider" min="0" max="100" value="50">
      </label>
      <label>
        Image Scale:
        <input type="range" id="scaleSlider" min="10" max="200" value="100">
      </label>
      <button id="newCurve">New curve</button>
      <button id="generatePikchr">Generate pikchr code</button>
      <button id="clearImage">Clear Background</button>
    </div>
    <svg id="editor" width="800" height="1200" style="border: 1px solid #ccc">
      <image id="backgroundImage" class="background-image" x="0" y="0" />
      <g id="curves"></g>
    </svg>
  </div>

  <div id="pikchr">
    <pre id="pikchr-code"></pre>
  </div>

  <script>
    class Curve {
      constructor(points = []) {
        this.points = points;
        this.element = document.createElementNS("http://www.w3.org/2000/svg", "g");
        this.path = document.createElementNS("http://www.w3.org/2000/svg", "path");
        this.path.setAttribute('class', 'curve');
        this.pointsGroup = document.createElementNS("http://www.w3.org/2000/svg", "g");
        this.midpointsGroup = document.createElementNS("http://www.w3.org/2000/svg", "g");

        this.element.appendChild(this.path);
        this.element.appendChild(this.pointsGroup);
        this.element.appendChild(this.midpointsGroup);
      }

      toJSON() {
        return {
          points: this.points
        };
      }

      static fromJSON(json) {
        return new Curve(json.points);
      }

      calculateMidpoints() {
        const midpoints = [];
        for (let i = 0; i < this.points.length - 1; i++) {
          midpoints.push({
            x: (this.points[i].x + this.points[i + 1].x) / 2,
            y: (this.points[i].y + this.points[i + 1].y) / 2
          });
        }
        return midpoints;
      }

      generatePath() {
        if (this.points.length < 2) return '';

        const midpoints = this.calculateMidpoints();
        let d = `M ${this.points[0].x} ${this.points[0].y}`;

        d += ` L ${midpoints[0].x} ${midpoints[0].y}`;

        for (let i = 1; i < this.points.length - 1; i++) {
          d += ` Q ${this.points[i].x} ${this.points[i].y}, ${midpoints[i].x} ${midpoints[i].y}`;
        }

        if (this.points.length > 1) {
          d += ` L ${this.points[this.points.length - 1].x} ${this.points[this.points.length - 1].y}`;
        }

        return d;
      }

      translate(dx, dy) {
        this.points.forEach(point => {
          point.x += dx;
          point.y += dy;
        });
        this.update();
      }

      clone() {
        return new Curve(this.points.map(p => ({x: p.x, y: p.y})));
      }

      update() {
        this.path.setAttribute('d', this.generatePath());

        this.pointsGroup.innerHTML = '';
        this.points.forEach((point, index) => {
          const circle = document.createElementNS("http://www.w3.org/2000/svg", "circle");
          circle.setAttribute('cx', point.x);
          circle.setAttribute('cy', point.y);
          circle.setAttribute('r', 3);
          circle.setAttribute('class', 'point');
          circle.dataset.index = index;
          circle.dataset.type = 'point';
          this.pointsGroup.appendChild(circle);
        });

        this.midpointsGroup.innerHTML = '';
        this.calculateMidpoints().forEach((point, index) => {
          const circle = document.createElementNS("http://www.w3.org/2000/svg", "circle");
          circle.setAttribute('cx', point.x);
          circle.setAttribute('cy', point.y);
          circle.setAttribute('r', 2);
          circle.setAttribute('class', 'midpoint');
          circle.dataset.index = index;
          circle.dataset.type = 'midpoint';
          this.midpointsGroup.appendChild(circle);
        });
      }
    }

    class UndoManager {
      constructor() {
        this.states = [];
        this.currentIndex = -1;
        this.maxStates = 50;
      }

      pushState(state) {
        // Remove any future states if we're not at the end
        this.states.splice(this.currentIndex + 1);

        // Add new state
        this.states.push(state);

        // Remove oldest state if we exceed maxStates
        if (this.states.length > this.maxStates) {
          this.states.shift();
        }

        this.currentIndex = this.states.length - 1;
      }

      undo() {
        if (this.currentIndex > 0) {
          this.currentIndex--;
          return this.states[this.currentIndex];
        }
        return null;
      }

      redo() {
        if (this.currentIndex < this.states.length - 1) {
          this.currentIndex++;
          return this.states[this.currentIndex];
        }
        return null;
      }
    }

    class CurveEditor {
      constructor(svgElement) {
        this.svg = svgElement;
        this.curves = [];
        this.selectedCurve = null;
        this.dragState = null;
        this.curvesContainer = document.getElementById('curves');
        this.undoManager = new UndoManager();
        this.currentDrawingName = '';

        this.setupEventListeners();
        this.newCurve();
      }

      setupEventListeners() {
        this.svg.addEventListener('dblclick', this.handleDblClick.bind(this));
        this.svg.addEventListener('mousedown', this.handleMouseDown.bind(this));
        document.addEventListener('mousemove', this.handleMouseMove.bind(this));
        document.addEventListener('mouseup', this.handleMouseUp.bind(this));
        document.addEventListener('keydown', this.handleKeyDown.bind(this));
        document.getElementById('newCurve').addEventListener('click', this.newCurve.bind(this));
        document.getElementById('generatePikchr').addEventListener('click', this.generatePikchr.bind(this));

        // Background image controls
        document.getElementById('imageInput').addEventListener('change', this.handleImageUpload.bind(this));
        document.getElementById('opacitySlider').addEventListener('input', this.updateImageOpacity.bind(this));
        document.getElementById('scaleSlider').addEventListener('input', this.updateImageScale.bind(this));
        document.getElementById('clearImage').addEventListener('click', this.clearBackgroundImage.bind(this));

        // Drawing management
        document.getElementById('saveDrawing').addEventListener('click', this.saveDrawing.bind(this));
        document.getElementById('loadDrawing').addEventListener('click', this.toggleDrawingsList.bind(this));
        document.getElementById('deleteDrawing').addEventListener('click', this.deleteDrawing.bind(this));

        // Initialize drawings list
        this.updateDrawingsList();
      }

      saveState() {
        const state = {
          curves: this.curves.map(curve => ({
            points: curve.points.map(p => ({x: p.x, y: p.y}))
          }))
        };
        this.undoManager.pushState(state);
      }

      restoreState(state) {
        if (!state) return;

        this.curvesContainer.innerHTML = '';
        this.curves = state.curves.map(curveData => {
          const curve = new Curve(curveData.points);
          this.curvesContainer.appendChild(curve.element);
          curve.update();
          return curve;
        });

        this.selectCurve(null);
      }

      newCurve() {
        this.addCurve([
          {x: 100, y: 200},
          {x: 200, y: 100},
          {x: 300, y: 300},
          {x: 400, y: 150}
        ]);
        this.saveState();
      }

      addCurve(points) {
        const curve = new Curve(points);
        this.curves.push(curve);
        this.curvesContainer.appendChild(curve.element);
        curve.update();
        this.selectCurve(curve);
      }

      selectCurve(curve) {
        if (this.selectedCurve) {
          this.selectedCurve.path.classList.remove('selected');
          this.selectedCurve.pointsGroup.classList.remove('selected');
          this.selectedCurve.midpointsGroup.classList.remove('selected');
        }
        this.selectedCurve = curve;
        if (curve) {
          curve.path.classList.add('selected');
          curve.pointsGroup.classList.add('selected');
          curve.midpointsGroup.classList.add('selected');
        }
      }

      handleMouseDown(e) {
        const rect = this.svg.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;

        if (e.target.dataset.type === 'point') {
          const curve = this.findCurveByElement(e.target.parentElement.parentElement);
          const index = parseInt(e.target.dataset.index);
          this.dragState = {
            type: 'point',
            curve,
            pointIndex: index,
            initialX: curve.points[index].x,
            initialY: curve.points[index].y,
            originalPoints: curve.points.map(p => ({x: p.x, y: p.y}))
          };
          this.selectCurve(curve);
        } else if (e.target.dataset.type === 'midpoint') {
          const curve = this.findCurveByElement(e.target.parentElement.parentElement);
          const index = parseInt(e.target.dataset.index);
          this.selectCurve(curve);
        } else if (e.target.classList.contains('curve')) {
          const curve = this.findCurveByElement(e.target.parentElement);
          this.dragState = {
            type: 'curve',
            curve,
            startX: x,
            startY: y,
            initialPoints: curve.points.map(p => ({x: p.x, y: p.y})),
            isCloning: e.altKey,
            originalCurve: curve,
            clonedCurve: null
          };

          if (e.altKey) {
            this.dragState.clonedCurve = curve.clone();
            this.curves.push(this.dragState.clonedCurve);
            this.curvesContainer.appendChild(this.dragState.clonedCurve.element);
            this.dragState.curve = this.dragState.clonedCurve;
          }

          this.selectCurve(this.dragState.curve);
        } else {
          this.selectCurve(null);
        }
      }

      handleMouseMove(e) {
        if (!this.dragState) return;

        const rect = this.svg.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;

        if (this.dragState.type === 'point') {
          const point = this.dragState.curve.points[this.dragState.pointIndex];
          point.x = x;
          point.y = y;
          this.dragState.curve.update();
        } else if (this.dragState.type === 'curve') {
          const dx = x - this.dragState.startX;
          const dy = y - this.dragState.startY;

          if (e.altKey && !this.dragState.isCloning) {
            this.dragState.clonedCurve = new Curve(this.dragState.initialPoints);
            this.curves.push(this.dragState.clonedCurve);
            this.curvesContainer.appendChild(this.dragState.clonedCurve.element);
            this.dragState.clonedCurve.update();
            this.dragState.isCloning = true;
          } else if (!e.altKey && this.dragState.isCloning) {
            this.curves = this.curves.filter(c => c !== this.dragState.clonedCurve);
            this.curvesContainer.removeChild(this.dragState.clonedCurve.element);
            this.dragState.clonedCurve = null;
            this.dragState.isCloning = false;
          }

          this.dragState.curve.points = this.dragState.initialPoints.map(p => ({
            x: p.x + dx,
            y: p.y + dy
          }));
          this.dragState.curve.update();
        }
      }

      handleMouseUp() {
        if (this.dragState) {
          // Save state after drag operations
          this.saveState();

          if (this.dragState.type === 'curve') {
            if (!this.dragState.isCloning && this.dragState.clonedCurve) {
              this.curves = this.curves.filter(c => c !== this.dragState.clonedCurve);
              this.curvesContainer.removeChild(this.dragState.clonedCurve.element);
            }
          }
        }
        this.dragState = null;
      }

      handleDblClick(e) {
        const rect = this.svg.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;

        if (e.target.dataset.type === 'point') {
          const curve = this.findCurveByElement(e.target.parentElement.parentElement);
          const index = parseInt(e.target.dataset.index);
          curve.points.splice(index, 1);
          if (curve.points.length === 0) {
            const curveIndex = this.curves.indexOf(curve);
            this.curves.splice(curveIndex, 1);
            this.curvesContainer.removeChild(curve.element);
            this.selectCurve(null);
          } else {
            curve.update();
          }
          this.saveState();
        } else if (e.target.dataset.type === 'midpoint') {
          const curve = this.findCurveByElement(e.target.parentElement.parentElement);
          const index = parseInt(e.target.dataset.index);
          curve.points.splice(index + 1, 0, {x, y});
          curve.update();
          this.selectCurve(curve);
          this.saveState();
        }
      }

      handleKeyDown(e) {
        // Handle undo/redo
        if ((e.metaKey || e.ctrlKey) && e.key === 'z') {
          if (e.shiftKey) {
            const state = this.undoManager.redo();
            if (state) this.restoreState(state);
          } else {
            const state = this.undoManager.undo();
            if (state) this.restoreState(state);
          }
          e.preventDefault();
          return;
        }

        if (e.key === 'Delete' || e.key === 'Backspace') {
          if (this.selectedCurve) {
            const index = this.curves.indexOf(this.selectedCurve);
            this.curves.splice(index, 1);
            this.curvesContainer.removeChild(this.selectedCurve.element);
            this.selectCurve(null);
            this.saveState();
          }
        }
      }

      findCurveByElement(element) {
        return this.curves.find(curve => curve.element === element);
      }

      handleImageUpload(e) {
        const file = e.target.files[0];
        if (file) {
          const reader = new FileReader();
          reader.onload = (event) => {
            const img = document.getElementById('backgroundImage');
            img.setAttribute('href', event.target.result);

            const tempImg = new Image();
            tempImg.onload = () => {
              img.dataset.originalWidth = tempImg.width;
              img.dataset.originalHeight = tempImg.height;
              this.updateImageScale();
            };
            tempImg.src = event.target.result;
          };
          reader.readAsDataURL(file);
        }
      }

      updateImageOpacity() {
        const opacity = document.getElementById('opacitySlider').value / 100;
        document.getElementById('backgroundImage').style.opacity = opacity;
      }

      updateImageScale() {
        const scale = document.getElementById('scaleSlider').value / 100;
        const img = document.getElementById('backgroundImage');
        const originalWidth = img.dataset.originalWidth;
        const originalHeight = img.dataset.originalHeight;

        if (originalWidth && originalHeight) {
          img.setAttribute('width', originalWidth * scale);
          img.setAttribute('height', originalHeight * scale);
        }
      }

      clearBackgroundImage() {
        const img = document.getElementById('backgroundImage');
        img.removeAttribute('href');
        img.removeAttribute('width');
        img.removeAttribute('height');
        document.getElementById('imageInput').value = '';
      }

      // Local Storage Management
      saveDrawing() {
        const name = document.getElementById('drawingName').value.trim();
        if (!name) {
          alert('Please enter a name for the drawing');
          return;
        }

        const drawing = {
          curves: this.curves.map(curve => curve.toJSON()),
          background: document.getElementById('backgroundImage').getAttribute('href'),
          originalWidth: document.getElementById('backgroundImage').dataset.originalWidth,
          originalHeight: document.getElementById('backgroundImage').dataset.originalHeight,
          opacity: document.getElementById('opacitySlider').value,
          scale: document.getElementById('scaleSlider').value
        };

        localStorage.setItem(`drawing_${name}`, JSON.stringify(drawing));
        this.currentDrawingName = name;
        this.updateDrawingsList();
      }

      loadDrawing(name) {
        const drawingJson = localStorage.getItem(`drawing_${name}`);
        if (!drawingJson) return;

        const drawing = JSON.parse(drawingJson);

        // Load curves
        this.curvesContainer.innerHTML = '';
        this.curves = drawing.curves.map(curveData => {
          const curve = Curve.fromJSON(curveData);
          this.curvesContainer.appendChild(curve.element);
          curve.update();
          return curve;
        });

        // Load background image
        const img = document.getElementById('backgroundImage');
        if (drawing.background) {
          img.setAttribute('href', drawing.background);
          img.dataset.originalWidth = drawing.originalWidth;
          img.dataset.originalHeight = drawing.originalHeight;
        } else {
          img.removeAttribute('href');
          img.removeAttribute('width');
          img.removeAttribute('height');
        }

        // Load settings
        document.getElementById('opacitySlider').value = drawing.opacity;
        document.getElementById('scaleSlider').value = drawing.scale;
        this.updateImageOpacity();
        this.updateImageScale();

        document.getElementById('drawingName').value = name;
        this.currentDrawingName = name;
        this.saveState();

        // Hide drawings list
        document.getElementById('drawingsList').style.display = 'none';
      }

      deleteDrawing() {
        const name = document.getElementById('drawingName').value.trim();
        if (!name) return;

        if (confirm(`Delete drawing "${name}"?`)) {
          localStorage.removeItem(`drawing_${name}`);
          if (name === this.currentDrawingName) {
            this.clearDrawing();
          }
          this.updateDrawingsList();
        }
      }

      clearDrawing() {
        this.curvesContainer.innerHTML = '';
        this.curves = [];
        this.clearBackgroundImage();
        document.getElementById('drawingName').value = '';
        this.currentDrawingName = '';
      }

      toggleDrawingsList() {
        const list = document.getElementById('drawingsList');
        list.style.display = list.style.display === 'none' ? 'block' : 'none';
      }

      updateDrawingsList() {
        const list = document.getElementById('drawingsList');
        list.innerHTML = '';

        const drawings = Object.keys(localStorage)
          .filter(key => key.startsWith('drawing_'))
          .map(key => key.replace('drawing_', ''));

        drawings.forEach(name => {
          const item = document.createElement('div');
          item.className = 'drawing-item';
          if (name === this.currentDrawingName) {
            item.classList.add('selected');
          }
          item.textContent = name;
          item.addEventListener('click', () => this.loadDrawing(name));
          list.appendChild(item);
        });
      }

      generatePikchr() {
        const pikchrDiv = document.getElementById('pikchr-code');

        if (this.curves.length == 0) {
          pikchrDiv.innerText = "# no curves";
          return;
        }

        var minX = Number.MAX_VALUE;
        var minY = Number.MAX_VALUE;
        var maxX = Number.MIN_VALUE;
        var maxY = Number.MIN_VALUE;

        this.curves.forEach(curve => {
          curve.points.forEach(point => {
            minX = Math.min(minX, point.x);
            minY = Math.min(minY, point.y);
            maxX = Math.max(maxX, point.x);
            maxY = Math.max(maxY, point.y);
          });
        });

        const factor = (maxX==minX) ? 1.0 : 4.0 / (maxX - minX);

        var lines = ['# splines'];

        const prec = (n, p) => {
          const s1 = n.toString();
          const s2 = n.toPrecision(p);
          const s3 = parseFloat(s2, 10).toString();
          var shortest = s1;
          shortest = (shortest.length <= s2.length) ? shortest : s2;
          shortest = (shortest.length <= s3.length) ? shortest : s3;
          return shortest;
        };

        const convert = (point) => {
          return `${prec((point.x-minX)*factor, 5)},${prec((maxY-point.y) * factor, 5)}`;
        };

        this.curves.forEach((curve, curveIndex) => {
          if (curve.points.length < 2) return;

          if (curveIndex > 0) {
            lines.push("");
          }

          if (curve.points.length == 2) {
            lines.push(`line from ${convert(curve.points[0])} to ${convert(curve.points[1])}`);
            return;
          }

          curve.points.forEach((point, i) => {
            if (i == 0) {
              lines.push(`spline from ${convert(point)} \\`);
            } else {
              const ending = (i == curve.points.length-1) ? "" : " \\";
              lines.push(`  then to ${convert(point)}${ending}`);
            }
          });
        });

        pikchrDiv.innerText = lines.join("\n");
      }
    }

    // Initialize the editor
    const editor = new CurveEditor(document.getElementById('editor'));
  </script>
