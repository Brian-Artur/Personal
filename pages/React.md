# Bucle de estado
	- No es acceso, es memoria + aviso
	- ![bucle_react.svg](../assets/bucle_react_1783793775209_0.svg)
- # Renderizado
	- **Render** :re-ejecute tu función componente. La corre de arriba abajo.
	- El **return** es el resultado esa ejecución; JSX.
	- ![Re-ejecuta.svg](../assets/Re-ejecuta_1783801868897_0.svg) 
	  Re-ejecuta el dueño del **estado** y sus descendientes.
	- ![image.png](../assets/image_1783805982544_0.png)
	- El diff compara descripción nueva vs anterior del Virtual DOM, no contra el DOM real.
	- `setState` no cierra la vuelta, arranca la siguiente.
- []