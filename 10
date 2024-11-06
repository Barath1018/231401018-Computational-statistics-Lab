import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
from sklearn.datasets import make_blobs

# 1. Generate synthesized data of size 200
X, _ = make_blobs(n_samples=200, centers=4, cluster_std=1.05, random_state=42)

# 2. Standardize the features
scaler = StandardScaler()
X_std = scaler.fit_transform(X)

# 3. Plot the original data
plt.figure(figsize=(6, 4))
plt.scatter(X[:, 0], X[:, 1], c='gray', s=50)
plt.title("Original Data Plot")
plt.xlabel("Feature 1")
plt.ylabel("Feature 2")
plt.show()

# 4. Plot the standardized features
plt.figure(figsize=(6, 4))
plt.scatter(X_std[:, 0], X_std[:, 1], c='gray', s=50)
plt.title("Standardized Data Plot")
plt.xlabel("Feature 1 (Standardized)")
plt.ylabel("Feature 2 (Standardized)")
plt.show()

# 5. Apply K-Means clustering with n=3
kmeans = KMeans(n_clusters=3, random_state=42)
y_kmeans = kmeans.fit_predict(X_std)

# 6. Plot clusters with center points for K=3
plt.figure(figsize=(6, 4))
plt.scatter(X_std[:, 0], X_std[:, 1], c=y_kmeans, s=50, cmap='viridis')
centers = kmeans.cluster_centers_
plt.scatter(centers[:, 0], centers[:, 1], c='red', s=200, alpha=0.75, marker='X')
plt.title("Clusters with K=3")
plt.xlabel("Feature 1 (Standardized)")
plt.ylabel("Feature 2 (Standardized)")
plt.show()

# 7. Find optimal K using the Elbow method
inertia = []
K_range = range(1, 10)
for k in K_range:
    kmeans = KMeans(n_clusters=k, random_state=42)
    kmeans.fit(X_std)
    inertia.append(kmeans.inertia_)

# 8. Plot the Elbow Graph
plt.figure(figsize=(6, 4))
plt.plot(K_range, inertia, 'bo-')
plt.title("Elbow Method for Optimal K")
plt.xlabel("Number of Clusters (K)")
plt.ylabel("Inertia")
plt.show()

# 9. Apply K-Means clustering with optimal K from the Elbow method (e.g., K=4)
optimal_k = 4
kmeans_optimal = KMeans(n_clusters=optimal_k, random_state=42)
y_kmeans_optimal = kmeans_optimal.fit_predict(X_std)

# 10. Plot clusters with center points for optimal K
plt.figure(figsize=(6, 4))
plt.scatter(X_std[:, 0], X_std[:, 1], c=y_kmeans_optimal, s=50, cmap='viridis')
centers_optimal = kmeans_optimal.cluster_centers_
plt.scatter(centers_optimal[:, 0], centers_optimal[:, 1], c='red', s=200, alpha=0.75, marker='X')
plt.title(f"Clusters with Optimal K={optimal_k}")
plt.xlabel("Feature 1 (Standardized)")
plt.ylabel("Feature 2 (Standardized)")
plt.show()
