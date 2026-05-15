# Fundamentos-de-programaci-n-UNAD
Fase 5-Evaluación final POA
#Albeiro Piza Calderon
#Grupo 2201
#Ingeniería electrónica
#Código fuente: autoría propia
 
productos = [
    ["Carne de Res",   "Cárnicos",          15000],
    ["Carne de Cerdo", "Cárnicos",          12000],
    ["Arroz",          "Granos",             2000],
    ["Lentejas",       "Granos",             3000],
    ["Coca-Cola 1.5L", "Bebidas Gaseosas",   6000],
    ["Coca-Cola 3L",   "Bebidas Gaseosas",  10000],
]
 
DESCUENTO_CARNICOS = 0.15
MINIMO_LIBRAS_DESC = 3
 

def obtener_categorias(productos):
    categorias = []
    for fila in productos:
        if fila[1] not in categorias:
            categorias.append(fila[1])
    return categorias
 
def filtrar_por_categoria(productos, categoria):
    return [p for p in productos if p[1] == categoria]
 
def pedir_numero(mensaje, maximo=None):
    while True:
        entrada = input(mensaje).strip()
        if entrada.isdigit() and int(entrada) > 0:
            numero = int(entrada)
            if maximo is None or numero <= maximo:
                return numero
            else:
                print(f"  ⚠️  Ingresa un número entre 1 y {maximo}.")
        else:
            print("  ⚠️  Entrada inválida. Ingresa un número entero mayor a 0.")
 
def pedir_si_no(mensaje):
    while True:
        entrada = input(mensaje).strip().lower()
        if entrada in ["s", "n"]:
            return entrada
        print("  ⚠️  Ingresa 's' para Sí o 'n' para No.")
 
def imprimir_factura(carrito):
    ancho = 58
    print("\n" + "=" * ancho)
    print("              🧾  FACTURA DE COMPRA")
    print("=" * ancho)
    print(f"  {'PRODUCTO':<20} {'CANT':>5} {'P.UNIT':>10} {'SUBTOTAL':>10}")
    print("-" * ancho)
 
    total_sin_descuento = 0
    total_descuentos    = 0
 
    for item in carrito:
        nombre    = item["nombre"]
        cantidad  = item["cantidad"]
        precio    = item["precio_unit"]
        subtotal  = item["subtotal"]
        descuento = item["descuento"]
 
        print(f"  {nombre:<20} {cantidad:>5} ${precio:>9,} ${subtotal:>9,}")
 
        if descuento > 0:
            print(f"  {'  🎉 Desc. 15% Cárnicos':<20} {'':>5} {'':>10} -${descuento:>8,.0f}")
 
        total_sin_descuento += subtotal
        total_descuentos    += descuento
 
    total_final = total_sin_descuento - total_descuentos
 
    print("-" * ancho)
    print(f"  {'SUBTOTAL':<38} ${total_sin_descuento:>10,}")
    if total_descuentos > 0:
        print(f"  {'DESCUENTOS APLICADOS':<38} -${total_descuentos:>9,.0f}")
    print("=" * ancho)
    print(f"  {'TOTAL A PAGAR':<38} ${total_final:>10,.0f}")
    print("=" * ancho)
 

carrito = []
 
print("\n" + "=" * 58)
print("          🛒  BIENVENIDO A LA TIENDA El INGE. 🛒")
print("=" * 58)
print("\n🔔 PROMOCIÓN: Más de 3 libras en CÁRNICOS → 15% descuento")
 

while True:
 
    # --- Categorías ---
    categorias = obtener_categorias(productos)
    print("\n📦 CATEGORÍAS DISPONIBLES:")
    print("-" * 32)
    for i, cat in enumerate(categorias, start=1):
        print(f"  {i}. {cat}")
    print("-" * 32)
 
    opcion_cat     = pedir_numero("\n👉 Selecciona la categoría: ", maximo=len(categorias))
    categoria_elegida = categorias[opcion_cat - 1]
 
    # --- Productos de la categoría ---
    productos_filtrados = filtrar_por_categoria(productos, categoria_elegida)
    print(f"\n🏷️  PRODUCTOS EN '{categoria_elegida.upper()}':")
    print("-" * 47)
    print(f"  {'#':<4} {'Producto':<22} {'Precio x libra':>13}")
    print("-" * 47)
    for i, prod in enumerate(productos_filtrados, start=1):
        print(f"  {i:<4} {prod[0]:<22} ${prod[2]:>12,}")
    print("-" * 47)
 
    opcion_prod    = pedir_numero("\n👉 Selecciona el producto: ", maximo=len(productos_filtrados))
    producto_elegido = productos_filtrados[opcion_prod - 1]
 
    cantidad = pedir_numero(f"\n👉 ¿Cuántas libras/unidades de '{producto_elegido[0]}'? ")
 
    #Calcular descuento
    subtotal  = producto_elegido[2] * cantidad
    aplica    = (producto_elegido[1] == "Cárnicos" and cantidad > MINIMO_LIBRAS_DESC)
    descuento = subtotal * DESCUENTO_CARNICOS if aplica else 0
 
    if aplica:
        print(f"\n  🎉 ¡Promoción aplicada! -{int(DESCUENTO_CARNICOS*100)}% en Cárnicos")
    elif producto_elegido[1] == "Cárnicos":
        print(f"\n  💡 Tip: Más de {MINIMO_LIBRAS_DESC} libras de Cárnicos = 15% descuento.")
 
    #Agregar al carrito
    carrito.append({
        "nombre"     : producto_elegido[0],
        "categoria"  : producto_elegido[1],
        "precio_unit": producto_elegido[2],
        "cantidad"   : cantidad,
        "subtotal"   : subtotal,
        "descuento"  : descuento,
    })
    print(f"\n  ✅ '{producto_elegido[0]}' x{cantidad} agregado al carrito.")
 
    # ¿Seguir comprando?
    respuesta = pedir_si_no("\n¿Deseas agregar otro producto? (s/n): ")
    if respuesta == "n":
        break
 

imprimir_factura(carrito)
