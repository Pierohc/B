    #S:

    import socket
    from threading import Thread
    import time
    
    
    def thread_leer_archivo(nombre_archivo : str):
        with open (nombre_archivo, "r") as f:
            contenido = f.read()
        t1.result = contenido
    
    
    def enviar_filas(arr_listas):
        client_socket.sendall(arr_listas.encode())
        print(f"Envié: {arr_listas}")
        
    
    
    
    if __name__ == '__main__':
    
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
        server_address = ('localhost', 5000)
        sock.bind(server_address)
        print("Iniciando el servidor...")
        sock.listen()
        print("Esperando conexiones...")
    
        i=1
    
        while True:
            client_socket, client_addres = sock.accept()
    
            try:
    
                t1 = Thread(target = thread_leer_archivo, args = ("PartesDeElectrónica.csv",))
                t1.start()
                t1.join()
                contenido = t1.result
                lineas = contenido.split("\n")
    
                if lineas[i] == "":
                    break
    
                t2 = Thread(target = enviar_filas, args = (lineas[i],))
                t2.start()
                t2.join()
                time.sleep(2.5)
    
            except KeyboardInterrupt:
                client_socket.close()
                break
    
            except IndexError:
                break
               
    
            finally:
                client_socket.close()
            
            i = i + 1


            #C------------------------


            import socket
            from threading import Thread
            
            def thread_recv_datos():
            
                data_b = sock.recv(1024)
                data = data_b.decode()
            
                t1.result = data
            
                
            def thread_process(fila,comp_alto_precio, comp_alto_precio_peso, comp_bajo_precio):
            
            
                    datos_lista = fila.split(";")
                    print(f"-----------------Nombre: {datos_lista[1]}------------------")
            
                    costo_total = float(datos_lista[5])*float(datos_lista[3])
                    print(f"Costo total: {costo_total}")
            
                    if costo_total < 25 :
                        print(f"Clasificacion por costo: Costo bajo")
                        comp_bajo_precio = comp_bajo_precio + 1
                    elif 25 <= costo_total <49.9:
                        print(f"Clasificacion por costo: Costo regular")
                    elif 50 <= costo_total < 74.9:
                        print(f"Clasificacion por costo: Costo alto")
                    elif costo_total >= 75:
                        print(f"Clasificacion por costo: Costo elevado")
                        comp_alto_precio = comp_alto_precio + 1 
                        if float(datos_lista[4])>= 100:
                            comp_alto_precio_peso = comp_alto_precio_peso + 1
            
                    print(f"Numero de componentes con costo elevado: {comp_alto_precio}")
                    print(f"Numero de componentes con costo elevado y con peso mayor a 100g: {comp_alto_precio_peso}")
                    print(f"Numero de componente con costo bajo: {comp_bajo_precio}")
            
                    t2.result = comp_alto_precio
                    t2.result2 = comp_alto_precio_peso
                    t2.result3 = comp_bajo_precio
            
            
            
            
            
            
            
            
            if __name__ == '__main__':
            
                
                comp_alto_precio = 0
                comp_alto_precio_peso = 0
                comp_bajo_precio = 0
                while True:
                    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                    server_address = ('localhost', 5000)
                    sock.connect(server_address)
                  
            
                    try:
            
                        t1 = Thread(target = thread_recv_datos, args = ())
                        t1.start()
                        t1.join()
                        fila = t1.result
            
                        if fila == "":
                            break
                        
            
                        print("\n")
                        t2 = Thread(target = thread_process, args = (fila, comp_alto_precio, comp_alto_precio_peso, comp_bajo_precio))
                        t2.start()
                        t2.join()
            
                        comp_alto_precio = t2.result
                        comp_alto_precio_peso = t2.result2
                        comp_bajo_precio = t2.result3
            
                    finally:
                        sock.close()
