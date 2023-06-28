    import time
    from werkzeug.security import check_password_hash
    from multiprocessing import Pool
    
    """
    Esta es la contraseña que usted tiene que adivinar. Está encriptada para que no pueda saber cuál es la respuesta correcta a priori.
    Lo que tiene que hacer es generar combinaciones de 3 letras y llamar a la función comparar_con_password_correcto(línea 24 de la plantilla)
    """
    contrasena_correcta = 'pbkdf2:sha256:260000$rTY0haIFRzP8wDDk$57d9f180198cecb45120b772c1317b561f390d677f3f76e36e0d02ac269ad224'
    
    
    # Arreglo con las letras del abecedario. Puede serle de ayuda, no es obligatorio que lo use
    abecedario = ['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z']
    
    """
    Función que sirve para comparar su palabra(cadena de 3 caracteres) con la contraseña correcta.
    Entrada: Su cadena de 3 caracteres
    Salida: True(verdadero) si es que coincide con la contraseña correcta, caso contrario retorna False(falso)
    """
    def comparar_con_password_correcto(palabra):
    	return check_password_hash(contrasena_correcta, palabra)
    
    #a):
    
    def generar_combinaciones():
        combinaciones = []
        for vocal1 in [letra for letra in abecedario if letra in 'aeiou']:
            for vocal2 in [letra for letra in abecedario if letra in 'aeiou']:
                for consonante in [letra for letra in abecedario if letra not in 'aeiou']:
                    combinacion = vocal1 + vocal2 + consonante
                    combinaciones.append(combinacion)
        return combinaciones
    
    def encontrar_contrasena():
        combinaciones = generar_combinaciones()
        for combinacion in combinaciones:
            if comparar_con_password_correcto(combinacion):
                return combinacion
            else:
                 print(f"No conindice {combinacion}")
    
    #b):
    
    def generar_combinaciones_multip(vocal1):
        combinaciones = []
        for vocal2 in [letra for letra in abecedario if letra in 'aeiou']:
            for consonante in [letra for letra in abecedario if letra not in 'aeiou']:
                combinacion = vocal1 + vocal2 + consonante
                combinaciones.append(combinacion)
        return combinaciones
    
    def encontrar_contrasena_multip(vocal1):
        combinaciones = generar_combinaciones_multip(vocal1)
        for combinacion in combinaciones:
            if comparar_con_password_correcto(combinacion):
                return combinacion
    
    
    if __name__ == "__main__":
            
        #a)
    	start_time = time.perf_counter()
    	contrasena_encontrada = encontrar_contrasena()
    	end_time = time.perf_counter()
    	tiempo_ejecucion = end_time - start_time
    
    	print("Contraseña encontrada:", contrasena_encontrada)
    	print("Tiempo de ejecución:", tiempo_ejecucion)
            
    
    	#b)
    	start_time2 = time.time()
    
    	vocales = [letra for letra in abecedario if letra in 'aeiou']
    	pool = Pool(processes=5)
    	contrasena_encontrada2 = pool.map(encontrar_contrasena_multip, vocales)
    	pool.close()
    	pool.join()
    
	end_time2 = time.time()
	tiempo_ejecucion2 = end_time2 - start_time2

	print("Contraseña encontrada:", contrasena_encontrada2)
	print("Tiempo de ejecución:", tiempo_ejecucion2)
