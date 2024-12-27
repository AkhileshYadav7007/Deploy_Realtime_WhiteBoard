# Deploy Realtime WhiteBoard
The Realtime WhiteBoard project is a collaborative web-based application that allows multiple users to draw and interact on a shared digital canvas in real-time. The platform mimics a physical whiteboard, making it an excellent tool for remote collaboration, brainstorming, or teaching.

Live Demo https://akhileshyadav7007.github.io/Deploy_Realtime_WhiteBoard/

### Simlpe WhiteBoard:
<img width="960" alt="image" src="https://github.com/user-attachments/assets/02450653-6726-4f06-be5b-14d1e992171b" />


### This Realtime WhiteBoard:
<img width="960" alt="image" src="https://github.com/user-attachments/assets/387fa78f-9c02-417d-af97-2470aee00cab" />


# Features
### Real-time Drawing: Multiple users can draw on the canvas simultaneously, with updates appearing in real-time for all participants.
### Pen Tool: Choose from different colors and pen sizes to draw freehand on the canvas.
### Eraser Tool: Erase unwanted parts of your drawing.
### Clear Canvas: Clear the entire canvas with a single click.
### Undo/Redo Functionality: Easily revert back or restore changes.
### Responsive Design: Works on a variety of screen sizes, from desktops to mobile devices.
# Technologies Used
### HTML5: Used to structure the web page and create the drawing canvas.
### CSS3: For styling the interface and making it responsive.
### JavaScript: For handling real-time drawing functionality and collaboration logic.
### Socket.io: Used to enable real-time communication between users.
### Node.js: Backend logic to manage multiple users and real-time events (if applicable).
### GitHub Pages: Used for hosting the live version of the app.
# Installation
To set up the project locally, follow these steps:

Clone the repository:
bash

Copy code
git clone https://github.com/AkhileshYadav7007/Deploy_Realtime_WhiteBoard.git
Navigate to the project directory:
bash

Copy code
cd Deploy_Realtime_WhiteBoard
Open index.html in your browser to run the application locally.
Usage

Open the project via the live demo link or run it locally.
Start drawing on the whiteboard using the pen tool. You can change the color and pen size using the provided options.
Use the eraser tool to remove any mistakes.

Use the undo and redo buttons to revert or restore changes.
Collaborate with other users in real-time by sharing the whiteboard link.

Logic of Drawing on the Canvas (One-to-One Explanation)
The core logic for drawing on the whiteboard involves handling mouse events (or touch events for mobile devices), detecting when the user starts drawing, moves the cursor, and stops drawing. The real-time collaboration is handled through WebSockets (Socket.io in this case), which transmits drawing data to other connected clients in real time.

Key Steps in the Logic
# Canvas Setup:

First, an HTML <canvas> element is used to create the drawing area. We set up the canvas size and context for drawing.
Example of canvas creation in HTML:

html
Copy code
<canvas id="whiteboard" width="800" height="600"></canvas>
# Mouse Events for Drawing:

When the user clicks on the canvas (mousedown), we detect the starting position and set a flag to start drawing.
As the user moves the mouse (mousemove), we keep drawing by connecting the previous and current mouse positions.
When the mouse is released (mouseup), drawing stops, and we can record the path drawn.
Here's the core drawing logic in JavaScript:

javascript
Copy code
const canvas = document.getElementById('whiteboard');
const ctx = canvas.getContext('2d');
let drawing = false;

// Function to start drawing
canvas.addEventListener('mousedown', (e) => {
  drawing = true;
  ctx.beginPath();
  ctx.moveTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
});

// Function to draw when moving the mouse
canvas.addEventListener('mousemove', (e) => {

 if (drawing) {
    ctx.lineTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
    ctx.stroke();
  }
});

// Function to stop drawing
canvas.addEventListener('mouseup', () => {
  drawing = false;
});

## Sending Data in Real-Time:

To enable real-time collaboration, the drawing data (such as the mouse position and path) needs to be shared with other connected users.
This is done using Socket.io, which broadcasts the drawing data to other users on the same whiteboard.

Example logic to send drawing data using Socket.io:

javascript
Copy code
      
      const socket = io(); // Initialize Socket.io connection


     canvas.addEventListener('mousemove', (e) => {
     
      
      if (drawing) {
      
    const x = e.clientX - canvas.offsetLeft;
    const y = e.clientY - canvas.offsetTop;
    
    // Emit drawing data to the server
    socket.emit('drawing', { x, y });

    ctx.lineTo(x, y);
    ctx.stroke();
    }
    });

// Listen for drawing data from other users
    socket.on('drawing', (data) => {
    
    ctx.lineTo(data.x, data.y);
    ctx.stroke();
    });

## Real-Time Broadcasting:

The server receives the drawing data from one client and broadcasts it to all other clients connected to the whiteboard.
Example server-side logic using Socket.io (Node.js):
javascript
Copy code

const io = require('socket.io')(server);

io.on('connection', (socket) => {
  console.log('A user connected');

  socket.on('drawing', (data) => {
    // Broadcast the drawing data to all connected clients
    socket.broadcast.emit('drawing', data);
  });
socket.on('disconnect', () => {
    console.log('A user disconnected');
  });
});

Contributing

Contributions are welcome! If you want to improve the project, add features, or fix bugs, follow these steps:

Fork the repository.
Create a new branch for your feature:
bash

Copy code
git checkout -b feature/AmazingFeature
Commit your changes:
bash

Copy code
git commit -m 'Add AmazingFeature'
Push to the branch:
bash

Copy code
git push origin feature/AmazingFeature
Open a pull request to merge your feature.
License
This project is open-source and available under the MIT License.
