# Ejemplo 1
import datetime
hoy = datetime.date.today()
print(hoy)

# Ejemplo 2
from datetime import datetime
ahora = datetime.now()
print(ahora)

# Ejemplo 3
from datetime import datetime
fecha_cadena = "2025-01-15 14:30:00"
fecha_objeto = datetime.strptime(fecha_cadena, "%Y-%m-%d %H:%M:%S")
print(fecha_objeto)

# Ejemplo 4
from datetime import datetime
fecha_formateada = fecha_objeto.strftime("%d/%m/%Y, %H:%M:%S")
print(fecha_formateada)

# Ejemplo 5
from datetime import date, timedelta
fecha_inicial = date(2025, 1, 15)
nueva_fecha = fecha_inicial + timedelta(days=5)
print(nueva_fecha)

# Ejemplo 6
from datetime import date, timedelta
fecha_menos = fecha_inicial - timedelta(days=10)
print(fecha_menos)

# Ejemplo 7
from datetime import datetime
diferencia = ahora - fecha_objeto
print(diferencia)

# Ejemplo 8
from datetime import datetime, time
solo_fecha = datetime(2025, 1, 15)
solo_tiempo = time(14, 30, 0)
combinada = datetime.combine(solo_fecha, solo_tiempo)
print(combinada)

# Ejemplo 9
from datetime import date
fecha_creada = date(2025, 12, 31)
print(fecha_creada)

# Ejemplo 10
from datetime import time
hora_creada = time(23, 59, 59)
print(hora_creada)

# Ejemplo 11
from datetime import date
dia_semana = fecha_inicial.weekday()  # Lunes=0, Domingo=6
print(dia_semana)

# Ejemplo 12
import datetime
dia_del_anio = fecha_inicial.timetuple().tm_yday
print(dia_del_anio)

# Ejemplo 13
from datetime import datetime
marca_tiempo = 1737004200
fecha_desde_timestamp = datetime.fromtimestamp(marca_tiempo)
print(fecha_desde_timestamp)

# Ejemplo 14
import time
inicio = time.time()
time.sleep(1)
fin = time.time()
print(fin - inicio)

# Ejemplo 15
from datetime import datetime, timezone
utc_now = datetime.now(timezone.utc)
print(utc_now)

# Ejemplo 16
print(utc_now.isoformat())

# Ejemplo 17
zona_horaria = datetime.now(timezone.utc).astimezone()
print(zona_horaria)

# Ejemplo 18
fecha_corta = fecha_inicial.strftime("%d-%m-%y")
print(fecha_corta)

# Ejemplo 19
from datetime import timedelta
dia_actual = date.today()
while dia_actual.weekday() != 0:  # 0: lunes
    dia_actual += timedelta(days=1)
print(dia_actual)

# Ejemplo 20
from datetime import datetime
fecha_anio_siguiente = datetime(2025, 1, 15) \
    .replace(year=datetime(2025, 1, 15).year + 1)
print(fecha_anio_siguiente)
