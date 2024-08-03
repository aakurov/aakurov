import pandas as pd
import numpy as np
from sklearn.cluster import DBSCAN
from scipy.spatial import ConvexHull
import matplotlib.pyplot as plt
import os

# Чтение данных из Excel-файла
file_path = r'C:\coordinates.xlsx'  # Используйте сырые строки для пути к файлу
df = pd.read_excel(file_path)

# Проверка названий столбцов
print("Columns in the Excel file:", df.columns)

# Убедитесь, что столбцы называются 'longitude' и 'latitude'
longitude_col = 'longitude'
latitude_col = 'latitude'

if longitude_col not in df.columns or latitude_col not in df.columns:
    print(f"Error: The columns '{longitude_col}' and '{latitude_col}' were not found in the Excel file.")
    exit()

# Извлечение координат из DataFrame
coordinates = df[[longitude_col, latitude_col]].to_numpy()

# Начальные значения параметров
eps_degrees = 0.01  # Примерно 1.1 км
min_samples = 10

# Кластеризация с использованием DBSCAN
clustering = DBSCAN(eps=eps_degrees, min_samples=min_samples).fit(coordinates)
labels = clustering.labels_

# Указание пути для сохранения файла
output_file = r'C:\Users\Андрей\OneDrive - ФГОБУ ВО Финансовый университет при Правительстве РФ\Рабочий стол\! Project - Contagion\clusters.txt'

# Подготовка к записи в файл
with open(output_file, 'w') as f:
    # Визуализация кластеров и выпуклых оболочек
    unique_labels = set(labels)
    for k in unique_labels:
        if k == -1:
            # Шум (outliers)
            continue
        
        class_member_mask = (labels == k)
        xy = coordinates[class_member_mask]
        
        # Запись координат кластера в файл
        f.write(f"Cluster {k}:\n")
        for point in xy:
            f.write(f"{point[0]}, {point[1]}\n")
        f.write("\n")

        # Печать координат кластера в консоль
        print(f"Cluster {k}:")
        for point in xy:
            print(f"{point[0]}, {point[1]}")
        print("\n")

# Оповещение о завершении записи
print(f"Координаты кластеров записаны в файл '{output_file}'")
