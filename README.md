# --- INSTALLATION ---
# Install the library for interactive mapping (Folium)
!pip install folium scikit-learn -q

# --- IMPORTS ---
import pandas as pd
import numpy as np
from sklearn.cluster import KMeans
import folium
import matplotlib.pyplot as plt

# --- STEP 1: GENERATE SYNTHETIC CUSTOMER LOCATION DATA ---
# Simulating 5,000 customer order locations in a dense urban area (e.g., Bangalore)
np.random.seed(42)
N = 5000

# Create three distinct 'hotspots' (simulating high-density residential/commercial zones)
def generate_cluster(center_lat, center_lon, num_points):
    latitudes = np.random.normal(center_lat, 0.015, num_points)
    longitudes = np.random.normal(center_lon, 0.015, num_points)
    return pd.DataFrame({'Latitude': latitudes, 'Longitude': longitudes})

df_locations = pd.concat([
    generate_cluster(12.9716, 77.5946, N // 3), # Hotspot A (e.g., Central Bangalore)
    generate_cluster(13.0379, 77.6416, N // 3), # Hotspot B (e.g., North-East Area)
    generate_cluster(12.9063, 77.6083, N - 2 * (N // 3)) # Hotspot C (e.g., South-East Area)
]).reset_index(drop=True)

print("✅ Geospatial Data Generated Successfully. Total Locations:", len(df_locations))
print(df_locations.head())

# --- STEP 2: APPLY K-MEANS CLUSTERING ---
# We use K=3, assuming we need 3 new Dark Stores to cover these hotspots.
# The centroid (center) of each cluster will be the optimal location.
X = df_locations[['Latitude', 'Longitude']]
K = 3 # Number of proposed Dark Stores

kmeans = KMeans(n_clusters=K, random_state=42, n_init=10)
df_locations['Cluster'] = kmeans.fit_predict(X)

# Extract the calculated optimal center points (Centroids)
centroids = kmeans.cluster_centers_
df_centroids = pd.DataFrame(centroids, columns=['Latitude', 'Longitude'])

print("\n--- OPTIMAL DARK STORE LOCATIONS (Centroids) ---")
print(df_centroids)

# --- STEP 3: VISUALIZATION (THE RECRUITER'S EYE-CANDY) ---
# Create an interactive Folium map centered around the data
m = folium.Map(location=[df_locations['Latitude'].mean(), df_locations['Longitude'].mean()], zoom_start=11)

# Add customer order locations (colored by cluster)
colors = ['blue', 'green', 'orange']
for i, row in df_locations.iterrows():
    folium.CircleMarker(
        location=[row['Latitude'], row['Longitude']],
        radius=3,
        color=colors[row['Cluster']],
        fill=True,
        fill_color=colors[row['Cluster']],
        fill_opacity=0.6
    ).add_to(m)

# Add the OPTIMAL Dark Store Locations (Large, Red Markers)
for i, row in df_centroids.iterrows():
    folium.Marker(
        location=[row['Latitude'], row['Longitude']],
        popup=f'Optimal Dark Store {i+1}',
        icon=folium.Icon(color='red', icon='warehouse')
    ).add_to(m)

# Save the map as HTML (You can upload this to your GitHub repository!)
map_file_path = 'dark_store_network_map.html'
m.save(map_file_path)

print(f"\n✅ Visualization Saved to: {map_file_path}")
print("Embed the map's screenshot in your README!")

# --- STEP 4: INVENTORY STRATEGY LOGIC (The Business Layer) ---
# Show how you link location to product strategy
cluster_inventory_mapping = {
    0: "High-Margin Snacks (Commercial Area Focus)",
    1: "Fresh Groceries (Residential Area Focus)",
    2: "Essentials & Beverages (High Traffic Focus)"
}

print("\n--- INVENTORY STRATEGY BY CLUSTER ---")
for i in range(K):
    print(f"Dark Store {i+1} (Cluster {i}): Inventory Focus -> {cluster_inventory_mapping[i]}")
