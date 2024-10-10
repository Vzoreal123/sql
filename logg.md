\

    import logging
    import socket
    import threading
    from flask import Flask, jsonify
    
    app = Flask(__name__)
    
    # Настройка UDP-логгера
    class UDPSocketHandler(logging.Handler):
        def __init__(self, host, port):
            super().__init__()
            self.address = (host, port)
            self.sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    
        def emit(self, record):
            msg = self.format(record)
            self.sock.sendto(msg.encode('utf-8'), self.address)
    
    # Настройка логгера
    logger = logging.getLogger('udp_logger')
    logger.setLevel(logging.DEBUG)
    
    udp_handler = UDPSocketHandler('localhost', 10000)  # Настройка на локальный адрес и порт
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    udp_handler.setFormatter(formatter)
    logger.addHandler(udp_handler)
    
    @app.route('/')
    def index():
        logger.info('Info level log')
        logger.debug('Debug level log')
        logger.warning('Warning level log')
        logger.error('Error level log')
        logger.critical('Critical level log')
        return jsonify({"message": "Logs sent!"})
    
    # Функция для запуска UDP-сервера
    def start_udp_server():
        udp_ip = 'localhost'
        udp_port = 10000
        sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        sock.bind((udp_ip, udp_port))
    
        print(f"Listening for logs on {udp_ip}:{udp_port}...")
        while True:
            data, addr = sock.recvfrom(1024)  # Приём данных
            print(f"Received log: {data.decode('utf-8')}")
    
    # Запуск Flask-приложения и UDP-сервера параллельно
    if __name__ == '__main__':
        # Запускаем UDP-сервер в отдельном потоке
        udp_thread = threading.Thread(target=start_udp_server)
        udp_thread.daemon = True  # Сделаем поток демоном, чтобы он завершался при закрытии программы
        udp_thread.start()
    
        # Запускаем Flask-приложение
        app.run(debug=True)
\

___[жирный курсив](http://localhost:5000/)___


