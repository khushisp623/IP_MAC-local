import socket
import uuid

def get_local_ip():
    # Get the local IP address
    local_ip = socket.gethostbyname(socket.gethostname())
    return local_ip

def get_mac_address():
    # Get the MAC address of the current device
    mac_address = ':'.join(['{:02x}'.format((uuid.getnode() >> elements) & 0xff) for elements in range(5, -1, -1)])
    return mac_address

def scan_network():
    local_ip = get_local_ip()
    mac_address = get_mac_address()

    print(f"Local IP Address: {local_ip}")
    print(f"MAC Address: {mac_address}")

    # Scan the local network
    for i in range(1, 255):
        ip = f"{local_ip.rsplit('.', 1)[0]}.{i}"
        try:
            # Attempt to connect to the device on the local network
            socket.create_connection((ip, 80), timeout=1)
            print(f"Device found - IP: {ip}")
        except (socket.timeout, ConnectionRefusedError):
            pass

if __name__ == "__main__":
    scan_network()
