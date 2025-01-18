# zerox
import socket
import os

# Set up the honeypot server
HOST = '0.0.0.0'  # Listen on all available interfaces
PORT = 12345      # Port to listen on

def create_honeypot():
    # Create a TCP/IP socket
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((HOST, PORT))
    server_socket.listen(5)  # Allow up to 5 connections
    print(f"Honeypot listening on {HOST}:{PORT}")

    while True:
        try:
            # Wait for a connection
            client_socket, addr = server_socket.accept()
            print(f"Connection from {addr}")
            handle_connection(client_socket)
        except Exception as e:
            print(f"Error: {e}")

def handle_connection(client_socket):
    # Inform the attacker they are connected
    client_socket.sendall(b"Connected to honeypot. Cannot send files here.\n")

    # Mimic receiving files without actually saving them
    while True:
        data = client_socket.recv(1024)
        if not data:
            break  # No more data, connection closed
        
        print("Received data (not saved):", data.decode('utf-8', errors='ignore'))
        
        # Sending back a message on every receive
        client_socket.sendall(b"File transfer is not allowed.\n")

    print("Closing connection.")
    client_socket.close()

if __name__ == "__main__":
    create_honeypot()
