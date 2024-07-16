import random
import math

# Haversine formula to calculate distance between two GPS coordinates
def haversine(lat1, lon1, lat2, lon2):
    R = 6371.0  # Earth radius in kilometers
    dlat = math.radians(lat2 - lat1)
    dlon = math.radians(lon2 - lon1)
    a = math.sin(dlat / 2)**2 + math.cos(math.radians(lat1)) * math.cos(math.radians(lat2)) * math.sin(dlon / 2)**2
    c = 2 * math.atan2(math.sqrt(a), math.sqrt(1 - a))
    distance = R * c
    return distance

# Generate random GPS data (lat, lon)
def generate_gps_data(n_points=10):
    gps_data = []
    lat, lon = 40.0, -74.0  # Starting point (example: somewhere in New York)
    for _ in range(n_points):
        lat += random.uniform(-0.01, 0.01)
        lon += random.uniform(-0.01, 0.01)
        gps_data.append((lat, lon))
    return gps_data

# Define a toll zone (circle with center and radius)
toll_zone_center = (40.01, -74.01)  # Example toll zone center
toll_zone_radius = 1.0  # Radius in kilometers

# Check if a point is within the toll zone
def is_within_toll_zone(lat, lon):
    distance = haversine(lat, lon, toll_zone_center[0], toll_zone_center[1])
    return distance <= toll_zone_radius

# Calculate toll based on GPS data
def calculate_toll(gps_data):
    toll = 0.0
    for lat, lon in gps_data:
        if is_within_toll_zone(lat, lon):
            toll += 1.0  # Example toll rate
    return toll

# Simulate the system
gps_data = generate_gps_data()
toll = calculate_toll(gps_data)

# Output results
print("GPS Data:", gps_data)
print("Total Toll:", toll)
