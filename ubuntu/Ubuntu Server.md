## Move files from remote ubuntu server to local pc

Open your terminal on your local machine

Use the following command structure to copy the folder:

```bash
scp -r username@remote_server:/home/api/Certificates /local/destination/path
```

 - `scp`: The secure copy command.
- `-r`: This option tells `scp` to copy directories recursively.
- `username`: Your username on the remote server.
- `remote_server`: The IP address or domain of your remote server.   
- `/home/api/Certificates`: The path to the folder on the remote server.
- `/local/destination/path`: The path where you want to copy the folder on your local machine.
- If the remote server uses a different port for SSH (other than the default port 22), you can 

specify the port with the `-P` option:

```bash
scp -P your_port -r username@remote_server:/home/api/Certificates /local/destination/path
```