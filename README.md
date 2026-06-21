EcoRoute
Sistema de control semafórico inteligente basado en aprendizaje por refuerzo profundo (Deep Q-Network) para la optimización simultánea de la fluidez vehicular y la reducción de emisiones de CO₂ en intersecciones urbanas simuladas.

Descripción
EcoRoute entrena un agente autónomo que aprende a controlar las fases semafóricas de una intersección urbana sin reglas explícitas, únicamente mediante interacción con un entorno de simulación. El agente observa el estado del tráfico en tiempo real y selecciona acciones que minimizan la congestión y la huella de carbono de forma simultánea.
El sistema utiliza el simulador SUMO como entorno de entrenamiento, una red neuronal profunda implementada en PyTorch como función de valor, y una función de recompensa híbrida que pondera tres objetivos:
R = −α · (tiempo de espera acumulado) − β · (emisiones CO₂) + γ · (throughput vehicular)
Los pesos α, β y γ son configurables, lo que permite explorar distintos compromisos entre movilidad y sostenibilidad sin modificar el código.

Motivación
El control semafórico convencional opera con ciclos de tiempo fijo diseñados para condiciones de tráfico promedio. Este enfoque es ineficiente ante flujos vehiculares variables y genera congestión innecesaria, mayor consumo de combustible y emisiones de CO₂ evitables. Los sistemas adaptativos comerciales existentes (SCOOT, SCATS) tienen costos de licencia prohibitivos para municipios de tamaño intermedio.
EcoRoute propone una alternativa open source basada en aprendizaje por refuerzo que se adapta dinámicamente al flujo real de tráfico y optimiza simultáneamente la movilidad y el impacto ambiental.

Características principales

Entorno de simulación integrado con SUMO vía interfaz TraCI compatible con Gymnasium
Agente DQN con Experience Replay, Target Network y política ε-greedy
Estimación de emisiones CO₂ con modelos VSP y COPERT intercambiables
Función de recompensa híbrida con pesos configurables desde YAML
Comparación automatizada contra baselines de tiempo fijo y adaptativo clásico
Análisis estadístico de resultados con prueba Mann-Whitney U
Reproducibilidad garantizada mediante semilla global configurable
Exportación de métricas a CSV y generación de gráficas en PNG y PDF


Requisitos
DependenciaVersión mínimaPython3.10SUMO1.18PyTorch2.0Gymnasium0.29NumPy1.24condaCualquiera

Instalación
bashgit clone https://github.com/[usuario]/ecoroute.git
cd ecoroute
conda env create -f environment.yml
conda activate ecoroute
Instalar SUMO según el sistema operativo siguiendo la documentación oficial en sumo.dlr.de.
Verificar la instalación:
bashpytest tests/ -v

Uso
Entrenar el agente:
bashpython train.py --config config/config.yaml
Evaluar el modelo entrenado:
bashpython evaluate.py --checkpoint results/checkpoints/checkpoint_ep500.pth --baselines
Ejecutar solo los baselines:
bashpython baseline.py --type fixed_time
python baseline.py --type adaptive_classic
Generar gráficas de resultados:
bashpython plot.py --experiment results/metrics/experimento_01.csv

Estructura del proyecto
ecoroute/
├── config/                  # Configuración centralizada del experimento
├── environment/             # Entorno SUMO, cálculo de emisiones y recompensa
├── agent/                   # Agente DQN, red neuronal y replay buffer
├── training/                # Loop de entrenamiento y controladores baseline
├── evaluation/              # Logger de métricas y generador de visualizaciones
├── sumo_files/              # Red vial, flujos y configuración de SUMO
├── tests/                   # Suite completa de pruebas automatizadas
├── results/                 # Checkpoints, métricas y gráficas (ignorado por Git)
├── config/config.yaml       # Parámetros del experimento
├── environment.yml          # Entorno conda reproducible
└── README.md

Reproducibilidad
Todos los experimentos son reproducibles fijando la semilla en config/config.yaml:
yamlsimulation:
  seed: 42
Dos ejecuciones con la misma semilla producen métricas idénticas episodio por episodio.

Licencia
MIT License — ver archivo LICENSE para los términos completos.
