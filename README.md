import socket
import ast

server=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
server.bind(("127.0.0.1",8080))
server.listen(1)

print("Server is running...")

conn,addr=server.accept()
data=conn.recv(4096).decode()
matrix=ast.literal_eval(data)

n=len(matrix)

if all(matrix[i][j]==0 for i in range(n) for j in range(n) if i!=j):
 result="Diagonal Matrix"
elif all(matrix[i][j]==0 for i in range(n) for j in range(i)):
 result="Upper Triangular Matrix"
elif all(matrix[i][j]==0 for i in range(n) for j in range(i+1,n)):
 result="Lower Triangular Matrix"
else:
 result="Neither"

conn.send(result.encode())
conn.close()
server.close()


import socket

client=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
client.connect(("127.0.0.1",8080))

matrix=input("Enter an N x N matrix as [[...],[...]]: ")

client.send(matrix.encode())

print("Result:",client.recv(1024).decode())
client.close()


