# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
## Server.py
```
import socket

s = socket.socket()
s.bind(("localhost", 8081))
s.listen(1)

print("Server running...")

while True:
    c, addr = s.accept()

    request = c.recv(1024).decode()
    print("Request received")

    if request.startswith("GET"):

        try:
            with open("index.html", "r") as f:
                data = f.read()

            response = "HTTP/1.1 200 OK\r\n\r\n" + data

        except FileNotFoundError:
            response = "HTTP/1.1 404 Not Found\r\n\r\nFile Not Found"

        c.send(response.encode())

    elif request.startswith("POST"):

        parts = request.split("\r\n\r\n", 1)

        if len(parts) > 1:
            data = parts[1]

            with open("upload.txt", "w") as f:
                f.write(data)

            response = "HTTP/1.1 200 OK\r\n\r\nFile Uploaded"
        else:
            response = "HTTP/1.1 400 Bad Request\r\n\r\nNo Data Received"

        c.send(response.encode())

    c.close()
```

## Client.py
```
import socket

s = socket.socket()
s.connect(("localhost", 8081))

ch = input("1.Download 2.Upload : ")

if ch == "1":
    req = "GET / HTTP/1.1\nHost: localhost\n\n"
    s.send(req.encode())

    data = s.recv(4096)
    print(data.decode())

else:
    msg = input("Enter data to upload: ")

    req = "POST / HTTP/1.1\nHost: localhost\n\n" + msg
    s.send(req.encode())

    data = s.recv(1024)
    print(data.decode())

s.close()
```

## index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <p>hello world</p>
</body>
</html>
```
## OUTPUT

## SERVER
<img width="503" height="64" alt="image" src="https://github.com/user-attachments/assets/8f37afcb-bea0-4d5b-85be-f2245500e0a9" />


## CLIENT
<img width="438" height="330" alt="image" src="https://github.com/user-attachments/assets/1d30b9b5-23e8-41d4-aed6-06e0e70338b6" />



## Result
Thus the socket for HTTP for web page upload and download created and Executed
