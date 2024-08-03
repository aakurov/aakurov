import numpy as np
from scipy.spatial import ConvexHull
import geojson
import os

# Вставьте свои координаты сюда
points = np.array([
    [0.1, 0.2],
    [0.3, 0.4],
    [0.5, 0.2],
    [0.7, 0.8],
    [0.9, 0.5]
])

# Нахождение выпуклой оболочки
hull = ConvexHull(points)
hull_points = points[hull.vertices]

# Указываем путь для сохранения файла
output_path = r'C:\Users\Андрей\OneDrive - ФГОБУ ВО Финансовый университет при Правительстве РФ\Рабочий стол\! Project - Contagion\convex_hull.geojson'

# Создание полигона GeoJSON
polygon = geojson.Polygon([hull_points.tolist()])

# Сохранение результата в файл GeoJSON
with open(output_path, 'w') as f:
    geojson.dump(polygon, f)

print(f"Выпуклая оболочка сохранена в файл '{output_path}'.")
