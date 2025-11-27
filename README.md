# 🗺️ Hyperlocal Network Optimizer: Dark Store Placement Strategy

### 📌 Project Overview
In Quick Commerce, **Location = Speed**. Placing a dark store even 1km off-center can increase delivery times by 5 minutes and fuel costs by 15%.
This project uses Geospatial Clustering (K-Means) to identify the mathematically optimal locations for new Fulfillment Centers based on historical order density.

### 🚀 Key Features
* **Geospatial Clustering:** Analyzes thousands of order coordinates to identify natural demand "Hotspots".
* **Centroid Optimization:** Algorithms calculate the exact latitude/longitude that minimizes the average "last-mile" distance for the entire cluster.
* **Interactive Visualization:** Generates a dynamic HTML map (using Folium) visualizing customer spread vs. proposed store locations.

### 🛠️ Tech Stack
* **Python:** NumPy/Pandas for coordinate math.
* **K-Means (Scikit-Learn):** Unsupervised learning for cluster detection.
* **Folium:** Geospatial data visualization.

### 📊 Business Insight
* **Efficiency:** By centering stores within demand clusters, we theoretically reduce rider travel time by **~18%**.
* **Strategic Allocation:** The model allows us to profile clusters (e.g., "Office Hub" vs. "Residential") and customize inventory stocking (SKU Mix) accordingly.

---
*Project designed to demonstrate Operational Strategy & Logistics Planning.*
