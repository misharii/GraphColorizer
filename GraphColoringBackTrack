import random
import tkinter as tk
from tkinter import ttk, messagebox
import networkx as netx
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg

def is_valid(graph, node, color, colors):
    """Checks if a color can be used for a node without breaking the rules."""
    for neighbor in graph.neighbors(node):
        if colors.get(neighbor) == color:
            return False
    return True

def solve_graph_coloring_backtrack(graph, node, m, colors):
    """Tries to color the graph using up to 'm' colors."""
    if node == len(graph.nodes):
        return True  # All nodes colored successfully, end recursion

    for color in range(1, m + 1):  # Try each color
        if is_valid(graph, node, color, colors):  # Check if current color is valid for the node
            colors[node] = color  # Assign color to the node
            if solve_graph_coloring_backtrack(graph, node + 1, m, colors):  # Move on to the next node
                return True  # Found valid coloring for all nodes, return True
            colors[node] = 0  # Reset color if no valid color can be found, backtrack

    return False  # No valid coloring found for this node with 'm' colors, return False

def find_min_colors_for_graph_coloring(graph):
    for m in range(1, len(graph.nodes) + 1):  # Start with 1 color and up to the number of nodes
        colors = {}
        if solve_graph_coloring_backtrack(graph, 0, m, colors):
            return m, colors
    return 0, {}

def visualize_graph(graph, colors):
    color_map = [colors.get(node, 0) for node in graph.nodes()]
    pos = netx.spring_layout(graph)
    netx.draw(graph, pos, node_color=color_map, with_labels=True, font_weight='bold')
    plt.show()

def validate_solution(graph, colors):
    for node in graph.nodes:
        for neighbor in graph.neighbors(node):
            if colors.get(node) == colors.get(neighbor):
                return False  # Adjacent nodes have the same color
    return True

def visualize_nodes_with_connections_and_colors(graph, colors):
    output_text = ""
    for node in graph.nodes():
        connected_nodes = list(graph.neighbors(node))
        output_text += "Node {} (Color: {}) is adjacent to: {}\n".format(
            node, colors.get(node, 'N/A'), connected_nodes)
    output_text += "---------------------------------------------\n"
    return output_text


def generate_random_graph(n):
    graph = netx.gnp_random_graph(n, 0.5, seed=random.randint(1, 100))
    return graph


def generate_random_graph(n):
    graph = netx.gnp_random_graph(n, 0.5, seed=random.randint(1, 100))
    return graph


def create_gui():
    root = tk.Tk()
    root.title("Graph Coloring Program")

    graph_visualization_frame = ttk.Frame(root)
    graph_visualization_frame.grid(column=0, row=6, columnspan=3, sticky=tk.EW)
    # Variables
    input_mode = tk.IntVar(value=0)  # 0 for manual, 1 for random
    num_of_nodes = tk.IntVar()
    edges = []  # List to store edges for manual mode

    ttk.Label(root, text="Choose input mode:").grid(column=0, row=0, sticky=tk.W)

    ttk.Label(root, text="Enter the number of nodes:").grid(column=0, row=1, sticky=tk.W)
    num_of_nodes_entry = ttk.Entry(root, textvariable=num_of_nodes)
    num_of_nodes_entry.grid(column=1, row=1, sticky=tk.EW)

    node_1_var, node_2_var = tk.StringVar(), tk.StringVar()
    node_1_menu = ttk.Combobox(root, textvariable=node_1_var, state="readonly")
    node_2_menu = ttk.Combobox(root, textvariable=node_2_var, state="readonly")

    edges_listbox_label = ttk.Label(root, text="Edges:")
    edges_listbox = tk.Listbox(root, height=5)

    def update_dropdowns():
        node_list = [str(n) for n in range(num_of_nodes.get())]
        node_1_menu['values'] = node_list
        node_2_menu['values'] = node_list

    # Function to add an edge
    def add_edge():
        node1 = node_1_var.get()
        node2 = node_2_var.get()
        if node1 != node2:
            edge = (int(node1), int(node2))
            if edge not in edges and (edge[1], edge[0]) not in edges:
                edges.append(edge)
                edges_listbox.insert(tk.END, f"{node1} - {node2}")
                node_1_var.set('')
                node_2_var.set('')
            else:
                messagebox.showerror("Error", "Edge already exists or is invalid.")
        else:
            messagebox.showerror("Error", "Cannot connect a node to itself.")

    add_edge_button = ttk.Button(root, text="Add Edge", command=add_edge)
    set_nodes_button = ttk.Button(root, text="Set Nodes", command=update_dropdowns)
    def toggle_input_mode():
        if input_mode.get() == 0:  # Manual mode
            node_1_menu.grid(column=0, row=3)
            node_2_menu.grid(column=1, row=3)
            add_edge_button.grid(column=2, row=3)  # Assuming you have defined this button outside to maintain reference
            edges_listbox_label.grid(column=0, row=4)
            edges_listbox.grid(column=0, row=5, columnspan=3, sticky=tk.W + tk.E)
            set_nodes_button.grid(column=2, row=1)  # Assuming you define this button outside as well
        else:  # Random mode
            node_1_menu.grid_remove()
            node_2_menu.grid_remove()
            add_edge_button.grid_remove()
            edges_listbox_label.grid_remove()
            edges_listbox.grid_remove()
            set_nodes_button.grid_remove()

    def reset_gui():
        edges.clear()
        edges_listbox.delete(0, tk.END)
        num_of_nodes.set(0)
        node_1_var.set('')
        node_2_var.set('')

        for widget in graph_visualization_frame.winfo_children():
            widget.destroy()

        node_1_menu['values'] = []
        node_2_menu['values'] = []

        input_mode.set(0)
        toggle_input_mode()

    ttk.Radiobutton(root, text="Manual", variable=input_mode, value=0, command=toggle_input_mode).grid(column=1, row=0)
    ttk.Radiobutton(root, text="Random", variable=input_mode, value=1, command=toggle_input_mode).grid(column=2, row=0)

    toggle_input_mode()

    def on_submit(messagebox=None):
        mode = input_mode.get()
        num_nodes = num_of_nodes.get()
        graph = netx.Graph()
        if mode == 0:  # Manual mode
            graph.add_nodes_from(range(num_nodes))
            graph.add_edges_from(edges)
        else:  # Random mode
            graph = generate_random_graph(num_nodes)

        min_colors, colors = find_min_colors_for_graph_coloring(graph)
        if colors:
            for widget in graph_visualization_frame.winfo_children():
                widget.destroy()
            node_info = visualize_nodes_with_connections_and_colors(graph, colors)
            output_text_widget.insert(tk.END, node_info + f"Minimum number of colors used: {min_colors}\n")
            color_map = [colors.get(node, 0) for node in graph.nodes()]
            pos = netx.spring_layout(graph)
            fig, ax = plt.subplots()
            netx.draw(graph, pos, node_color=color_map, with_labels=True, font_weight='bold', ax=ax)

            canvas = FigureCanvasTkAgg(fig, master=graph_visualization_frame)  # A tk.DrawingArea.
            canvas.draw()
            canvas.get_tk_widget().pack()


        else:
            messagebox.showinfo("Result", "No valid coloring found with the given constraints.")

    output_text_widget = tk.Text(root, height=10, width=50)
    output_text_widget.grid(column=0, row=8, columnspan=3, padx=5, pady=5, sticky="ew")

    # Submit Button
    ttk.Button(root, text="Color Graph", command=on_submit).grid(column=0, row=7, sticky=tk.W + tk.E, padx=5, pady=5)
    ttk.Button(root, text="Reset", command=reset_gui).grid(column=1, row=7, sticky=tk.W + tk.E, padx=5, pady=5)

    root.mainloop()

if __name__ == "__main__":
    create_gui()
