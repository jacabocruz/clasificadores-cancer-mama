Clasificadores aplicados al cáncer de mama

Comparación de cinco algoritmos de clasificación (Árbol de Decisión, Random Forest, SVM, K-NN y Naive Bayes Gaussiano) con el conjunto Breast Cancer Wisconsin (Diagnostic) usando scikit-learn.

El propósito es académico: observar cómo aprende, decide y se equivoca cada algoritmo bajo las mismas condiciones, no declarar un ganador. Los modelos no son una herramienta de diagnóstico clínico.

Contenido
clasificadores-cancer-mama/
├── clasificacion_cancer_mama.ipynb
├── Informe_clasificadores_cancer_mama_BohorquezJamer.pdf
├── requirements.txt
└── README.md

Si tu notebook aún se llama agrupacion.ipynb, renómbralo a clasificacion_cancer_mama.ipynb.

Datos
Fuente: sklearn.datasets.load_breast_cancer, basado en Wolberg, Street y Mangasarian (1995), UCI Machine Learning Repository.
Tamaño: 569 observaciones y 30 variables numéricas (características de los núcleos celulares).
Variable objetivo: se recodifica para que 1 = maligno (212 casos) y 0 = benigno (357 casos).
Reglas del experimento
Misma división entrenamiento/prueba para los cinco modelos.
80 % entrenamiento y 20 % prueba, con random_state=42 y stratify=y.
El conjunto de prueba no se modifica durante la comparación.
Escalamiento (StandardScaler en un pipeline) solo para SVM y K-NN.
Mismas métricas para todos: Accuracy, Precision, Recall, F1-score y matriz de confusión.
No se elige el modelo solo por Accuracy: se interpretan los errores y el contexto.
Modelos y parámetros
Modelo	Parámetros	Escalamiento
Árbol de Decisión	max_depth=4, random_state=42	No
Random Forest	n_estimators=100, max_depth=4, random_state=42	No
SVM	kernel='rbf', C=1.0, gamma='scale'	Sí
K-NN	n_neighbors=5	Sí
Naive Bayes Gaussiano	Valores por defecto	No
Resultados

Conjunto de prueba de 114 registros (72 benignos y 42 malignos).

Modelo	Accuracy	VN	FP	FN	VP	Recall (maligno)	Precision (maligno)
SVM	0.9737	72	0	3	39	0.929	1.000
Random Forest	0.9649	72	0	4	38	0.905	1.000
K-NN	0.9561	71	1	4	38	0.905	0.974
Naive Bayes	0.9386	72	0	7	35	0.833	1.000
Árbol de Decisión	0.9123	70	2	8	34	0.810	0.944

VN = verdaderos negativos, FP = falsos positivos, FN = falsos negativos, VP = verdaderos positivos. El Recall y la Precision de la clase maligna se calcularon a partir de las matrices de confusión.

Nota sobre las métricas del notebook: la función de evaluación usa average='weighted', por lo que el Recall impreso coincide con el Accuracy y no describe la detección de la clase maligna. Para medir la clase maligna directamente, cambia a average='binary' (valor por defecto).

Conclusiones principales
El SVM obtuvo el mejor desempeño y el menor número de falsos negativos (3). Su ventaja sobre Random Forest es de un solo caso, por lo que no es concluyente.
En un diagnóstico el error más costoso es el falso negativo, por eso se prioriza el Recall de la clase maligna sobre el Accuracy.
Naive Bayes tuvo Precision de 1.000 y aun así dejó pasar 7 tumores malignos: una sola métrica puede engañar.
Hay un compromiso entre desempeño e interpretabilidad: el Árbol de Decisión es el más fácil de explicar y el que más casos malignos dejó pasar.
Limitaciones
Una única partición de prueba de 114 registros.
Hiperparámetros sin ajustar (por ejemplo, max_depth=4 y k=5 no se compararon con otros valores).
No se ejecutó SVM ni K-NN sin escalar, así que el efecto del escalamiento no se midió.
Cómo ejecutarlo
bash
git clone https://github.com/<usuario>/clasificadores-cancer-mama.git
cd clasificadores-cancer-mama
python -m venv venv
source venv/bin/activate        # En Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook clasificacion_cancer_mama.ipynb

Probado con Python 3.11.

Referencias
Pedregosa, F., et al. (2011). Scikit-learn: Machine learning in Python. Journal of Machine Learning Research, 12, 2825-2830.
McKinney, W. (2010). Data structures for statistical computing in Python. Proceedings of the 9th Python in Science Conference, 56-61.
Street, W. N., Wolberg, W. H., & Mangasarian, O. L. (1993). Nuclear feature extraction for breast tumor diagnosis. Proceedings of SPIE, 1905, 861-870.
Wolberg, W. H., Street, W. N., & Mangasarian, O. L. (1995). Breast Cancer Wisconsin (Diagnostic) [Data set]. UCI Machine Learning Repository. https://doi.org/10.24432/C5DW2B
Autor

Jamer Camilo Bohórquez Cruz
