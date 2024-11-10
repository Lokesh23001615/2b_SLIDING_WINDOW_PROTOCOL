# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
# Name: Lokesh M
# Reg no: 212223230114
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
client.py
```
import socket

s = socket.socket()
s.connect(('localhost', 8000))

while True:
    # Receive data from server (frames)
    data = s.recv(1024).decode()
    if not data:
        break  # Exit loop if no data received (server disconnected)
    print(f"Received frames: {data}")
    
    # Acknowledge receipt of the frames
    s.send("Acknowledgement received".encode())

s.close()  # Close the connection when done

```
server.py
```

import socket

s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)

print("Waiting for a connection...")
c, addr = s.accept()
print(f"Connected to {addr}")

# Input number of frames and window size
size = int(input("Enter the number of frames to send: "))
frames = list(range(size))
window_size = int(input("Enter Window Size: "))

start = 0

while start < size:
    end = min(start + window_size, size)  # Ensure not to go beyond frame size
    
    # Send frames within the window
    frames_to_send = frames[start:end]
    c.send(str(frames_to_send).encode())
    
    # Wait for acknowledgment from the client
    ack = c.recv(1024).decode()
    if ack:
        print(f"Client acknowledgment: {ack}")
    
    # Move the window
    start += window_size

# Close the connection when done
c.close()
s.close()

```
## OUPUT
![image](https://github.com/user-attachments/assets/244e36d2-5681-48f6-8488-85e1ffac2b22)

## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
