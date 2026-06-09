# Dashboard de Producción — Zeruk Brokers

Dashboard Flask + MySQL para análisis de producción sobre la tabla `zeruk.DashbordLk`.

## Métricas
- Fee Neto USD (`DashbordLkFeeNetoUSD`)
- Prima Neta USD (`DashbordLkPrimaNetaUSD`)
- Comisión Zyra USD (`DashbordLkMCZyraUSD`)
- Comisión Producer USD (`DashbordLkMCProducerUSD`)
- Cantidad de registros

## Funcionalidades
- Filtro por **Inicio de Vigencia** con presets (mes actual/anterior, trimestre, YTD, año anterior, últimos 30 días, últimos 12 meses, personalizado).
- **Comparativo automático vs período anterior**: si el rango son meses calendario completos, compara contra los meses previos equivalentes; si no, contra el rango inmediatamente anterior de la misma duración.
- Filtros por Aseguradora, Producer, Ramo y Tipo de Venta.
- Desglose por pestañas: Aseguradora · Producer · Ramo · Razón Social · Tipo de Venta, con orden por columna y exportación a CSV.
- Gráfico de evolución mensual (Prima Neta + Fee Neto) y Top 10 por dimensión.
- Auto-refresco cada 5 minutos con contador, botón de actualización manual.
- `color-scheme: light only` (compatible con el navegador del TV Miray).

## Ejecución local (Windows)
```powershell
cd dashboard-produccion
pip install -r requirements.txt
python app.py
```
Abrir http://localhost:5000

## Despliegue en Render
1. Subir la carpeta a un repositorio de GitHub.
2. En Render: **New → Web Service**, conectar el repo.
3. Build Command: `pip install -r requirements.txt`
4. Start Command: `gunicorn app:app`
5. **Environment Variables** (recomendado, no dejar credenciales en el código):
   - `DB_HOST` = 34.125.156.253
   - `DB_USER` = mantenedor
   - `DB_PASSWORD` = (la contraseña)
   - `DB_NAME` = zeruk

## Embebido en Zyra (iframe)
```html
<iframe src="https://TU-SERVICIO.onrender.com" style="width:100%;height:100vh;border:none;"></iframe>
```

## Notas técnicas
- Conexión por request con timeouts (connect 10s, read/write 30s) — el servidor MySQL
  no responde el handshake sin timeout configurado, por eso son obligatorios.
- Dimensiones validadas contra whitelist y consultas parametrizadas (sin inyección SQL).
- Zona horaria America/Lima para "hoy" y el sello de actualización.
