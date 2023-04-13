1) Make anothe folder named server outside the client folder and paste the below code

const server = 5000;
const io = require('socket.io')(server, {
    cors: {
      origin: '*',
    }
});

io.on('connection', socket => {
  const id = socket.handshake.query.id
  socket.join(id)

  socket.on('send-message', ({ recipients, text }) => {
    recipients.forEach(recipient => {
      const newRecipients = recipients.filter(r => r !== recipient)
      newRecipients.push(id)
      socket.broadcast.to(recipient).emit('receive-message', {
        recipients: newRecipients, sender: id, text
      })
    })
  })
})

2) Now install socket.io and nodemon in server folder and start you node server

3) Run npm start in the client folder to start the Chat Application