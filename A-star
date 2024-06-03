import streamlit as st
import heapq
import pandas as pd

class Graph:
    def _init_(self):
        self.edges = {}
        self.heuristics = {}

    def add_edge(self, from_node, to_node, weight):
        if from_node not in self.edges:
            self.edges[from_node] = []
        self.edges[from_node].append((to_node, weight))
        if to_node not in self.edges:
            self.edges[to_node] = []
        self.edges[to_node].append((from_node, weight))

    def set_heuristic(self, node, value):
        self.heuristics[node] = value

    def heuristic(self, node, goal):
        return self.heuristics.get(node, 0)

    def a_star(self, start, goal):
        open_set = []
        heapq.heappush(open_set, (0, start))
        came_from = {}
        g_score = {node: float('inf') for node in self.edges}
        g_score[start] = 0
        f_score = {node: float('inf') for node in self.edges}
        f_score[start] = self.heuristic(start, goal)

        while open_set:
            _, current = heapq.heappop(open_set)

            if current == goal:
                path = self.reconstruct_path(came_from, start, goal)
                return path, g_score[goal]

            for neighbor, weight in self.edges.get(current, []):
                tentative_g_score = g_score[current] + weight
                if tentative_g_score < g_score[neighbor]:
                    came_from[neighbor] = current
                    g_score[neighbor] = tentative_g_score
                    f_score[neighbor] = g_score[neighbor] + self.heuristic(neighbor, goal)
                    heapq.heappush(open_set, (f_score[neighbor], neighbor))

        return None, float('inf')

    def reconstruct_path(self, came_from, start, goal):
        path = [goal]
        while goal in came_from:
            goal = came_from[goal]
            path.append(goal)
        path.reverse()
        return path

#Inisialisasi graf
graph = Graph()

# Menambahkan edge atau jarak
edges = [
    ('A', 'B', 4.157), ('A', 'C', 3.072), ('A', 'H', 3.015), ('A', 'J', 3.040),
    ('B', 'C', 2.070), ('B', 'D', 1.958), ('C', 'D', 1.538), ('D', 'E', 1.921),
    ('E', 'F', 1.472), ('E', 'G', 2.051), ('F', 'G', 0.926), ('H', 'I', 1.675),
    ('I', 'J', 2.271)
]
for edge in edges:
    graph.add_edge(*edge)

# Menambahkan nilai heuristic
heuristics = {
    'A': 2500, 'B': 2000, 'C': 1500, 'D': 1000,
    'E': 2000, 'F': 0, 'G': 500, 'H': 6000,
    'I': 2000, 'J': 2800
}
for node, value in heuristics.items():
    graph.set_heuristic(node, value)

# Membuat dataframe yang akan menampilkan nilai heuristic pada streamlit sebagai tabel
heuristics_df = pd.DataFrame(list(heuristics.items()), columns=['Node', 'Heuristic'])
heuristics_df['Location'] = heuristics_df['Node'].map({
    'A': 'Kantor Pos Pusat Medan',
    'B': 'Komplek Bandar Selamat Permai',
    'C': 'Kantor Pos Bakaran Batu',
    'D': 'Kantor Pos Laksamana',
    'E': 'Kantor Pos Menteng',
    'F': 'Kantor Pos SM. Raja Medan',
    'G': 'Kantor Pos Alfalah',
    'H': 'Kantor Pos Medan Baru',
    'I': 'Kantor Pos Gatot Subroto',
    'J': 'Kantor Pos Tengku Amir Hamza'
})

# Judul pada streamlit
st.title('PENCARIAN JALUR TERPENDEK PENGIRIMAN BARANG MENGGUNAKAN ALGORITMA A*')

# Menampilkan tabel
st.subheader('Nilai Heuristik')
st.table(heuristics_df[['Location', 'Heuristic']])

# Mapping untuk display nama
location_map = {
    'Kantor Pos Pusat Medan': 'A',
    'Komplek Bandar Selamat Permai': 'B',
    'Kantor Pos Bakaran Batu': 'C',
    'Kantor Pos Laksamana': 'D',
    'Kantor Pos Menteng': 'E',
    'Kantor Pos SM. Raja Medan': 'F',
    'Kantor Pos Alfalah': 'G',
    'Kantor Pos Medan Baru': 'H',
    'Kantor Pos Gatot Subroto': 'I',
    'Kantor Pos Tengku Amir Hamza': 'J'
}

# Mendapatkan user input
start_location = st.selectbox('Pilih Awal', list(location_map.keys()))
goal_location = st.selectbox('Pilih Tujuan', list(location_map.keys()))

# Mengkonversi nama lokasi menjadi node
start_node = location_map[start_location]
goal_node = location_map[goal_location]

# Melakukan algoritma pencarian A*
if st.button('Cari'):
    path, cost = graph.a_star(start_node, goal_node)
    if path:
        path_locations = [key for key, value in location_map.items() if value in path]
        result = f"Langkah: {' -> '.join(path_locations)}\n\n*Jarak:* {cost:.3f} km"
    else:
        result = f"Tidak ada langkah yang ditemukan dari {start_location} ke {goal_location}"

    st.markdown(
        f"""
        <div style="padding: 10px; border: 1px solid gray; border-radius: 5px; background-color: #f9f9f9; color: #333;">
            {result}
        </div>
        """,
        unsafe_allow_html=True
    )
