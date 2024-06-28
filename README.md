# Transport-Network-in-the-City-of-Bern

import networkx as nx
import matplotlib.pyplot as plt

# Adjazenzmatrix als 2D-Liste
adjacency_matrix = [
    [0, 295, 149, 285, 352, 113, 277, 160, 109, 0, 400],
    [312, 0, 0, 0, 0, 0, 0, 0, 0, 143, 45],
    [130, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [335, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [358, 0, 0, 0, 0, 0, 64, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 115],
    [265, 0, 0, 0, 65, 0, 0, 0, 0, 0, 64],
    [170, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [114, 0, 0, 0, 0, 0, 0, 0, 0, 0, 58],
    [0, 157, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [536, 48, 0, 0, 0, 0, 64, 0, 59, 0, 0]
]
# Bahnhofsnamen zuordnen
station_names = {
    0: "Bern",
    1: "Europaplatz",
    2: "Längasse",
    3: "Neufeld P&R",
    4: "Wankdorf",
    5: "Elfenau",
    6: "Ostermundigen",
    7: "Wabern",
    8: "Weissenbühl",
    9: "Bümpliz",
    10: "Brunnadernstrasse"
}

# Koordinaten für die Knoten festlegen (Beispielkoordinaten)
node_positions = {
    0: (600062, 199849),
    1: (597632, 199216),
    2: (599057, 200563),
    3: (599500, 201329),
    4: (601959, 201721),
    5: (602594, 197948),
    6: (603299, 200527),
    7: (599816, 198422),
    8: (599100, 198100),
    9: (596322, 199400),
    10: (601914, 198903)
}
# Erstellen eines gerichteten Graphen aus der Adjazenzmatrix
G = nx.DiGraph()

# Hinzufügen von Knoten und Kanten zum Graphen
for i in range(len(adjacency_matrix)):
    G.add_node(i)  # Knoten hinzufügen
    for j in range(len(adjacency_matrix[i])):
        if adjacency_matrix[i][j] != 0:
            G.add_edge(i, j, weight=adjacency_matrix[i][j])  # Kante hinzufügen

# Berechnung der Degree-Zentralität für jeden Bahnhof
degree_centrality = nx.degree_centrality(G)
# Größe der Knoten basierend auf der Degree-Zentralität
node_size_degree = [round(centrality * 1000,3) for centrality in degree_centrality.values()]
# Graphen zeichnen
plt.figure(figsize=(12, 8))
nx.draw(G, pos=node_positions, with_labels=False, node_size=node_size_degree, node_color='skyblue', edge_color='lightblue', arrowsize=20, width=2)

# Station Namen und Degree Centrality hinzufügen
for node, (x, y) in node_positions.items():
    name = station_names[node]
    degree = round(degree_centrality[node], 3)
    plt.text(x, y, f"{name}\nDegree: {degree}", fontsize=10, ha='center', va='center')

plt.show()


# Closeness-Zentralität für jeden Knoten berechnen
closeness_centrality = nx.closeness_centrality(G)
# Größe der Knoten basierend auf der Closeness-Zentralität
node_size = [round(centrality * 1000, 3) for centrality in closeness_centrality.values()]

# Graphen zeichnen
plt.figure(figsize=(12, 8))
nx.draw(G, pos=node_positions, with_labels=False, node_size=node_size, node_color='skyblue', edge_color='lightblue', arrowsize=20, width=2)

# Knotenbeschriftungen hinzufügen
for node, (x, y) in node_positions.items():
    plt.text(x, y, f"{station_names[node]}\nCloseness: {round(closeness_centrality[node], 3)}", fontsize=10, ha='center', va='center', bbox=dict(facecolor='white', alpha=0.5, edgecolor='none'))

plt.title("Closeness-Centrality")
plt.axis('off')
plt.tight_layout()
plt.show()

# Graphen zeichnen
plt.figure(figsize=(12, 8))
nx.draw(G, pos=node_positions, with_labels=False, node_size=node_size, node_color='skyblue', edge_color='lightblue', arrowsize=20, width=2)

# Knotenbeschriftungen hinzufügen
for node, (x, y) in node_positions.items():
    plt.text(x, y, station_names[node], fontsize=10, ha='center', va='center')

plt.title("Standorte")
plt.axis('off')
plt.tight_layout()
plt.show()



# Calculating the eigenvector centrality for each station
eigenvector_centrality = nx.eigenvector_centrality(G)
# Node size based on eigenvector centrality
node_size_eigenvector = [round(centrality * 1000, 3) for centrality in eigenvector_centrality.values()]

# Drawing the graph with both degree centrality and eigenvector centrality
plt.figure(figsize=(12, 8))
nx.draw(G, pos=node_positions, with_labels=False, node_size=node_size_degree, node_color='lightblue', edge_color='lightblue', arrowsize=20, width=2, alpha=0.5)

# Adding station names and degree centrality
for node, (x, y) in node_positions.items():
    name = station_names[node]
    degree = round(degree_centrality[node], 3)
    plt.text(x, y, f"{name}\nDegree: {degree}", fontsize=10, ha='center', va='center', color='blue')

# Drawing nodes with eigenvector centrality as red circles
nx.draw_networkx_nodes(G, pos=node_positions, node_size=node_size_eigenvector, node_color='red')

plt.title("Network with Eigenvector Centrality")
plt.axis('off')
plt.tight_layout()
plt.show()


# Calculate the number of edges for each node
num_edges = {node: len(G.out_edges(node)) for node in G.nodes()}

# Print the result
for node, num_edge in num_edges.items():
    print(f"Node {node}: {num_edge} edges")
