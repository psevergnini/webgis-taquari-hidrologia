          🛰️ Sentinel-1 (inundação real)
                     │
          🌧️ CEMADEN / INMET (chuva)
                     │
          📡 Radar meteorológico (nowcasting)
                     │
                     ▼
            ETL STREAM (Node/Kafka)
                     │
                     ▼
        🌊 HEC-HMS → vazão → HEC-RAS
                     │
                     ▼
        🧠 IA FUSION MODEL (calibração)
                     │
                     ▼
        🗺️ PostGIS + GeoServer
                     │
                     ▼
        🌐 WebGIS / App / Alertas
        Radar → chuva → sensores → Sentinel
          ↓
     normalização temporal
          ↓
       stream buffer
       setInterval(async () => {

  const chuva = await getCemaden()
  const radar = await getRadar()
  const hec = await runHECRAS()

  emit("hydro-stream", {
    chuva,
    radar,
    hec
  })

}, 3000)
X =
[
  chuva_1h,
  chuva_6h,
  radar_intensidade,
  radar_movimento,
  hec_ras_nivel,
  hec_ras_vazao,
  sentinel_flood_area
]
y =
[
  nivel_real_futuro,
  risco_inundacao,
  probabilidade_transbordamento
]
from xgboost import XGBRegressor

model = XGBRegressor(
    n_estimators=400,
    max_depth=6,
    learning_rate=0.05
)

model.fit(X, y)
imagem SAR antes
        ↓
imagem SAR depois
        ↓
diferença (Δ backscatter)
        ↓
mapa de inundação real
flood = (after < -17) & (before > -15)
vetor vento → desloca célula de chuva → previsão imediata
erro = sentinel_real - hec_ras_simulado
nivel_corrigido = hec_ras + IA(erro, radar, chuva)
            🌧️ chuva
               │
📡 radar → previsão imediata
               │
🛰️ sentinel → realidade observada
               │
🌊 HEC-RAS → simulação física
               │
               ▼
        🧠 IA de fusão
               │
               ▼
     🗺️ WebGIS operacional
               │
               ▼
     🚨 alertas automáticos
     
