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
import socket

def send_request(host, port, request):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request)

        response = b""
        while True:
            chunk = s.recv(4096)
            if not chunk:
                break
            response += chunk
    return response


def upload_file(host, port, filename):
    with open(filename, 'rb') as file:
        file_data = file.read()

    content_length = len(file_data)

    # Simple raw HTTP POST (not multipart)
    header = (
        f"POST /upload HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Content-Length: {content_length}\r\n"
        f"Connection: close\r\n\r\n"
    ).encode()

    request = header + file_data
    response = send_request(host, port, request)

    # Strip headers
    if b"\r\n\r\n" in response:
        _, body = response.split(b"\r\n\r\n", 1)
    else:
        body = response

    return body.decode(errors="ignore")


def download_file(host, port, filename):
    request = (
        f"GET /{filename} HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Connection: close\r\n\r\n"
    ).encode()

    response = send_request(host, port, request)

    # Split into headers + body
    if b"\r\n\r\n" in response:
        _, body = response.split(b"\r\n\r\n", 1)
    else:
        body = response

    # Save file
    with open(filename, 'wb') as file:
        file.write(body)


if _name_ == "_main_":
    host = 'example.com'
    port = 80

    upload_response = upload_file(host, port, 'example.txt')
    print("Upload response:", upload_response)

    download_file(host, port, 'example.txt')
    print("File downloaded successfully.")
## OUTPUT
<img width="1918" height="1128" alt="image" src="https://github.com/user-attachments/assets/3dc7dfd4-6708-441d-bba9-bf520dc4879a" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
