# 590012069-Sem-2-University-Python-Project
# Advanced Mathematics Calculator

![Calculator Screenshot](screenshot.png) <!-- Add a screenshot if available -->

A comprehensive GUI-based mathematics calculator application built with Python and Tkinter, featuring algebraic, calculus, matrix, and plotting capabilities.

## Features

- **Basic Calculator**
  - Standard arithmetic operations
  - Scientific functions (sin, cos, tan, log, etc.)
  - Constants (π, e)
  - Calculation history

- **Algebra Operations**
  - Equation solving
  - Expression simplification
  - Polynomial factoring
  - Expression expansion

- **Calculus Operations**
  - Symbolic differentiation
  - Integration (definite and indefinite)
  - Limit calculation

- **Function Plotting**
  - 2D function graphs
  - Multiple function plotting
  - Customizable ranges
  - Graph titles

- **Matrix Operations**
  - Matrix addition/subtraction
  - Matrix multiplication
  - Determinant calculation
  - Matrix inversion
  - Transpose
  - Eigenvalue calculation

## Technologies Used

- Python 3.x
- Tkinter (GUI)
- NumPy (Matrix operations)
- SymPy (Symbolic mathematics)
- Matplotlib (Plotting)

## Code

   ```python
import tkinter as tk
from tkinter import ttk, scrolledtext, messagebox
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
import sympy as sp
import math
from sympy import symbols, diff, integrate, solve, simplify, Matrix, limit, oo
import re
class MathCalculator(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Advanced Mathematics Calculator")
        self.geometry("1200x800")
        self.configure(bg="#f0f0f0")
        self.create_widgets()
    def create_widgets(self):
        # Set up main frames
        self.sidebar = tk.Frame(self, width=300, bg="#2c3e50")
        self.sidebar.pack(side=tk.LEFT, fill=tk.Y)
        self.sidebar.pack_propagate(False)
        self.main_content = tk.Frame(self, bg="#f0f0f0")
        self.main_content.pack(side=tk.RIGHT, fill=tk.BOTH, expand=True)
        # Sidebar title
        sidebar_title = tk.Label(
            self.sidebar,
            text="Functions",
            font=("Arial", 16, "bold"),
            bg="#2c3e50",
            fg="white",
            pady=10,
        )
        sidebar_title.pack(fill=tk.X)
        # Create notebook for different function categories
        self.notebook = ttk.Notebook(self.main_content)
        self.notebook.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)
        # Create tabs
        self.basic_tab = tk.Frame(self.notebook, bg="#f0f0f0")
        self.algebra_tab = tk.Frame(self.notebook, bg="#f0f0f0")
        self.calculus_tab = tk.Frame(self.notebook, bg="#f0f0f0")
        self.plotting_tab = tk.Frame(self.notebook, bg="#f0f0f0")
        self.matrix_tab = tk.Frame(self.notebook, bg="#f0f0f0")
        self.notebook.add(self.basic_tab, text="Basic")
        self.notebook.add(self.algebra_tab, text="Algebra")
        self.notebook.add(self.calculus_tab, text="Calculus")
        self.notebook.add(self.plotting_tab, text="Plotting")
        self.notebook.add(self.matrix_tab, text="Matrix")
        # Create sidebar buttons
        button_styles = {
            "bg": "#34495e",
            "fg": "white",
            "font": ("Arial", 12),
            "bd": 0,
            "pady": 10,
            "padx": 5,
            "width": 25,
            "activebackground": "#1abc9c",
            "activeforeground": "white",
        }
        # Basic functions
        basic_functions_label = tk.Label(
            self.sidebar,
            text="Basic Functions",
            bg="#2c3e50",
            fg="#1abc9c",
            font=("Arial", 12, "bold"),
        )
        basic_functions_label.pack(fill=tk.X, pady=(20, 5))
        self.create_sidebar_button(
            "Standard Calculator", lambda: self.notebook.select(0), **button_styles
        )
        self.create_sidebar_button(
            "Scientific Functions", lambda: self.notebook.select(0), **button_styles
        )
        # Algebra functions
        algebra_functions_label = tk.Label(
            self.sidebar,
            text="Algebra Functions",
            bg="#2c3e50",
            fg="#1abc9c",
            font=("Arial", 12, "bold"),
        )
        algebra_functions_label.pack(fill=tk.X, pady=(20, 5))
        self.create_sidebar_button(
            "Solve Equations", lambda: self.notebook.select(1), **button_styles
        )
        self.create_sidebar_button(
            "Simplify Expressions", lambda: self.notebook.select(1), **button_styles
        )
        # Calculus functions
        calculus_functions_label = tk.Label(
            self.sidebar,
            text="Calculus Functions",
            bg="#2c3e50",
            fg="#1abc9c",
            font=("Arial", 12, "bold"),
        )
        calculus_functions_label.pack(fill=tk.X, pady=(20, 5))
        self.create_sidebar_button(
            "Differentiation", lambda: self.notebook.select(2), **button_styles
        )
        self.create_sidebar_button(
            "Integration", lambda: self.notebook.select(2), **button_styles
        )
        self.create_sidebar_button(
            "Limits", lambda: self.notebook.select(2), **button_styles
        )
        # Plotting functions
        plotting_functions_label = tk.Label(
            self.sidebar,
            text="Plotting Functions",
            bg="#2c3e50",
            fg="#1abc9c",
            font=("Arial", 12, "bold"),
        )
        plotting_functions_label.pack(fill=tk.X, pady=(20, 5))
        self.create_sidebar_button(
            "2D Function Plot", lambda: self.notebook.select(3), **button_styles
        )
        # Matrix functions
        matrix_functions_label = tk.Label(
            self.sidebar,
            text="Matrix Operations",
            bg="#2c3e50",
            fg="#1abc9c",
            font=("Arial", 12, "bold"),
        )
        matrix_functions_label.pack(fill=tk.X, pady=(20, 5))
        self.create_sidebar_button(
            "Matrix Calculator", lambda: self.notebook.select(4), **button_styles
        )
        # Set up the content for each tab
        self.setup_basic_tab()
        self.setup_algebra_tab()
        self.setup_calculus_tab()
        self.setup_plotting_tab()
        self.setup_matrix_tab()
    def create_sidebar_button(self, text, command, **kwargs):
        btn = tk.Button(self.sidebar, text=text, command=command, **kwargs)
        btn.pack(fill=tk.X)
        return btn
    def setup_basic_tab(self):
        # Left panel for input
        left_panel = tk.Frame(self.basic_tab, bg="#f0f0f0")
        left_panel.pack(side=tk.LEFT, fill=tk.BOTH, expand=True, padx=10, pady=10)
        # Right panel for history
        right_panel = tk.Frame(self.basic_tab, bg="#f0f0f0", width=300)
        right_panel.pack(side=tk.RIGHT, fill=tk.Y, padx=10, pady=10)
        right_panel.pack_propagate(False)
        # Calculator display
        display_frame = tk.Frame(left_panel, bg="#f0f0f0")
        display_frame.pack(fill=tk.X, pady=10)
        self.entry_var = tk.StringVar()
        self.entry = tk.Entry(
            display_frame,
            textvariable=self.entry_var,
            font=("Arial", 24),
            bd=10,
            relief=tk.GROOVE,
            justify=tk.RIGHT,
        )
        self.entry.pack(fill=tk.X)
        # Calculator buttons
        button_frame = tk.Frame(left_panel, bg="#f0f0f0")
        button_frame.pack(fill=tk.BOTH, expand=True)
        # Define button layout
        buttons = [
            ("7", 1, 0),
            ("8", 1, 1),
            ("9", 1, 2),
            ("/", 1, 3),
            ("C", 1, 4),
            ("4", 2, 0),
            ("5", 2, 1),
            ("6", 2, 2),
            ("*", 2, 3),
            ("AC", 2, 4),
            ("1", 3, 0),
            ("2", 3, 1),
            ("3", 3, 2),
            ("-", 3, 3),
            ("(", 3, 4),
            ("0", 4, 0),
            (".", 4, 1),
            ("=", 4, 2),
            ("+", 4, 3),
            (")", 4, 4),
            ("sin", 5, 0),
            ("cos", 5, 1),
            ("tan", 5, 2),
            ("log", 5, 3),
            ("ln", 5, 4),
            ("π", 6, 0),
            ("e", 6, 1),
            ("√", 6, 2),
            ("^", 6, 3),
            ("!", 6, 4),
        ]
        # Create buttons
        for text, row, col in buttons:
            if text == "=":
                btn = tk.Button(
                    button_frame,
                    text=text,
                    font=("Arial", 18),
                    bg="#1abc9c",
                    fg="white",
                    padx=20,
                    pady=20,
                    command=self.calculate,
                )
            elif text in ("C", "AC"):
                btn = tk.Button(
                    button_frame,
                    text=text,
                    font=("Arial", 18),
                    bg="#e74c3c",
                    fg="white",
                    padx=20,
                    pady=20,
                    command=lambda t=text: self.clear(t),
                )
            elif text in ("+", "-", "*", "/", "^"):
                btn = tk.Button(
                    button_frame,
                    text=text,
                    font=("Arial", 18),
                    bg="#34495e",
                    fg="white",
                    padx=20,
                    pady=20,
                    command=lambda t=text: self.add_to_expression(t),
                )
            elif text in ("sin", "cos", "tan", "log", "ln", "√", "!"):
                btn = tk.Button(
                    button_frame,
                    text=text,
                    font=("Arial", 18),
                    bg="#3498db",
                    fg="white",
                    padx=20,
                    pady=20,
                    command=lambda t=text: self.add_function(t),
                )
            elif text in ("π", "e"):
                btn = tk.Button(
                    button_frame,
                    text=text,
                    font=("Arial", 18),
                    bg="#9b59b6",
                    fg="white",
                    padx=20,
                    pady=20,
                    command=lambda t=text: self.add_constant(t),
                )
            else:
                btn = tk.Button(
                    button_frame,
                    text=text,
                    font=("Arial", 18),
                    bg="#ecf0f1",
                    fg="#2c3e50",
                    padx=20,
                    pady=20,
                    command=lambda t=text: self.add_to_expression(t),
                )
            btn.grid(row=row, column=col, padx=5, pady=5, sticky="nsew")
        # Configure grid weights
        for i in range(7):
            button_frame.grid_rowconfigure(i, weight=1)
        for i in range(5):
            button_frame.grid_columnconfigure(i, weight=1)
        # History section
        history_label = tk.Label(
            right_panel, text="History", font=("Arial", 14, "bold"), bg="#f0f0f0"
        )
        history_label.pack(pady=(0, 10))
        self.history_text = scrolledtext.ScrolledText(
            right_panel, width=25, height=20, font=("Arial", 12)
        )
        self.history_text.pack(fill=tk.BOTH, expand=True)
        self.history_text.config(state=tk.DISABLED)
    def setup_algebra_tab(self):
        # Create frames
        left_panel = tk.Frame(self.algebra_tab, bg="#f0f0f0")
        left_panel.pack(side=tk.LEFT, fill=tk.BOTH, expand=True, padx=10, pady=10)
        right_panel = tk.Frame(self.algebra_tab, bg="#f0f0f0", width=400)
        right_panel.pack(side=tk.RIGHT, fill=tk.Y, padx=10, pady=10)
        right_panel.pack_propagate(False)
        # Left panel content
        left_title = tk.Label(
            left_panel,
            text="Algebraic Operations",
            font=("Arial", 16, "bold"),
            bg="#f0f0f0",
        )
        left_title.pack(pady=10)
        # Expression input
        expr_frame = tk.Frame(left_panel, bg="#f0f0f0")
        expr_frame.pack(fill=tk.X, pady=10)
        expr_label = tk.Label(
            expr_frame, text="Expression:", font=("Arial", 12), bg="#f0f0f0"
        )
        expr_label.pack(side=tk.LEFT, padx=5)
        self.algebra_expr = tk.Entry(expr_frame, font=("Arial", 12), width=40)
        self.algebra_expr.pack(side=tk.LEFT, padx=5, fill=tk.X, expand=True)
        self.algebra_expr.insert(0, "x**2 + 2*x + 1")
        # Variable input
        var_frame = tk.Frame(left_panel, bg="#f0f0f0")
        var_frame.pack(fill=tk.X, pady=10)
        var_label = tk.Label(
            var_frame, text="Variable(s):", font=("Arial", 12), bg="#f0f0f0"
        )
        var_label.pack(side=tk.LEFT, padx=5)
        self.algebra_var = tk.Entry(var_frame, font=("Arial", 12), width=20)
        self.algebra_var.pack(side=tk.LEFT, padx=5)
        self.algebra_var.insert(0, "x")
        # Operation buttons
        btn_frame = tk.Frame(left_panel, bg="#f0f0f0")
        btn_frame.pack(fill=tk.X, pady=10)
        solve_btn = tk.Button(
            btn_frame,
            text="Solve",
            font=("Arial", 12),
            bg="#3498db",
            fg="white",
            padx=15,
            pady=5,
            command=self.solve_equation,
        )
        solve_btn.pack(side=tk.LEFT, padx=5)
        simplify_btn = tk.Button(
            btn_frame,
            text="Simplify",
            font=("Arial", 12),
            bg="#3498db",
            fg="white",
            padx=15,
            pady=5,
            command=self.simplify_expression,
        )
        simplify_btn.pack(side=tk.LEFT, padx=5)
        factor_btn = tk.Button(
            btn_frame,
            text="Factor",
            font=("Arial", 12),
            bg="#3498db",
            fg="white",
            padx=15,
            pady=5,
            command=self.factor_expression,
        )
        factor_btn.pack(side=tk.LEFT, padx=5)
        expand_btn = tk.Button(
            btn_frame,
            text="Expand",
            font=("Arial", 12),
            bg="#3498db",
            fg="white",
            padx=15,
            pady=5,
            command=self.expand_expression,
        )
        expand_btn.pack(side=tk.LEFT, padx=5)
        # Right panel content - results
        right_title = tk.Label(
            right_panel, text="Results", font=("Arial", 16, "bold"), bg="#f0f0f0"
        )
        right_title.pack(pady=10)
        self.algebra_result = scrolledtext.ScrolledText(
            right_panel, width=30, height=20, font=("Arial", 12)
        )
        self.algebra_result.pack(fill=tk.BOTH, expand=True)
    def setup_calculus_tab(self):
        # Create frames
        left_panel = tk.Frame(self.calculus_tab, bg="#f0f0f0")
        left_panel.pack(side=tk.LEFT, fill=tk.BOTH, expand=True, padx=10, pady=10)
        right_panel = tk.Frame(self.calculus_tab, bg="#f0f0f0", width=400)
        right_panel.pack(side=tk.RIGHT, fill=tk.Y, padx=10, pady=10)
        right_panel.pack_propagate(False)
        # Left panel content
        left_title = tk.Label(
            left_panel,
            text="Calculus Operations",
            font=("Arial", 16, "bold"),
            bg="#f0f0f0",
        )
        left_title.pack(pady=10)
        # Expression input
        expr_frame = tk.Frame(left_panel, bg="#f0f0f0")
        expr_frame.pack(fill=tk.X, pady=10)
        expr_label = tk.Label(
            expr_frame, text="Expression:", font=("Arial", 12), bg="#f0f0f0"
        )
        expr_label.pack(side=tk.LEFT, padx=5)
        self.calculus_expr = tk.Entry(expr_frame, font=("Arial", 12), width=40)
        self.calculus_expr.pack(side=tk.LEFT, padx=5, fill=tk.X, expand=True)
        self.calculus_expr.insert(0, "x**2 + sin(x)")
        # Variable input
        var_frame = tk.Frame(left_panel, bg="#f0f0f0")
        var_frame.pack(fill=tk.X, pady=10)
        var_label = tk.Label(
            var_frame, text="Variable:", font=("Arial", 12), bg="#f0f0f0"
        )
        var_label.pack(side=tk.LEFT, padx=5)
        self.calculus_var = tk.Entry(var_frame, font=("Arial", 12), width=10)
        self.calculus_var.pack(side=tk.LEFT, padx=5)
        self.calculus_var.insert(0, "x")
        # Limit point for limit calculation
        limit_frame = tk.Frame(left_panel, bg="#f0f0f0")
        limit_frame.pack(fill=tk.X, pady=10)
        limit_label = tk.Label(
            limit_frame, text="Limit Point:", font=("Arial", 12), bg="#f0f0f0"
        )
        limit_label.pack(side=tk.LEFT, padx=5)
        self.limit_point = tk.Entry(limit_frame, font=("Arial", 12), width=10)
        self.limit_point.pack(side=tk.LEFT, padx=5)
        self.limit_point.insert(0, "0")
        # Integration limits
        int_frame = tk.Frame(left_panel, bg="#f0f0f0")
        int_frame.pack(fill=tk.X, pady=10)
        from_label = tk.Label(
            int_frame, text="Integrate from:", font=("Arial", 12), bg="#f0f0f0"
        )
        from_label.grid(row=0, column=0, padx=5, pady=5)
        self.int_from = tk.Entry(int_frame, font=("Arial", 12), width=10)
        self.int_from.grid(row=0, column=1, padx=5, pady=5)
        self.int_from.insert(0, "0")
        to_label = tk.Label(int_frame, text="to:", font=("Arial", 12), bg="#f0f0f0")
        to_label.grid(row=0, column=2, padx=5, pady=5)
        self.int_to = tk.Entry(int_frame, font=("Arial", 12), width=10)
        self.int_to.grid(row=0, column=3, padx=5, pady=5)
        self.int_to.insert(0, "1")
        # Operation buttons
        btn_frame = tk.Frame(left_panel, bg="#f0f0f0")
        btn_frame.pack(fill=tk.X, pady=10)
        diff_btn = tk.Button(
            btn_frame,
            text="Differentiate",
            font=("Arial", 12),
            bg="#3498db",
            fg="white",
            padx=15,
            pady=5,
            command=self.differentiate,
        )
        diff_btn.pack(side=tk.LEFT, padx=5)
        int_btn = tk.Button(
            btn_frame,
            text="Integrate",
            font=("Arial", 12),
            bg="#3498db",
            fg="white",
            padx=15,
            pady=5,
            command=self.integrate,
        )
        int_btn.pack(side=tk.LEFT, padx=5)
        def_int_btn = tk.Button(
            btn_frame,
            text="Definite Integral",
            font=("Arial", 12),
            bg="#3498db",
            fg="white",
            padx=15,
            pady=5,
            command=self.definite_integral,
        )
        def_int_btn.pack(side=tk.LEFT, padx=5)
        limit_btn = tk.Button(
            btn_frame,
            text="Calculate Limit",
            font=("Arial", 12),
            bg="#3498db",
            fg="white",
            padx=15,
            pady=5,
            command=self.calculate_limit,
        )
        limit_btn.pack(side=tk.LEFT, padx=5)
        # Right panel content - results
        right_title = tk.Label(
            right_panel, text="Results", font=("Arial", 16, "bold"), bg="#f0f0f0"
        )
        right_title.pack(pady=10)
        self.calculus_result = scrolledtext.ScrolledText(
            right_panel, width=30, height=20, font=("Arial", 12)
        )
        self.calculus_result.pack(fill=tk.BOTH, expand=True)
    def setup_plotting_tab(self):
        # Create frames
        left_panel = tk.Frame(self.plotting_tab, bg="#f0f0f0")
        left_panel.pack(side=tk.LEFT, fill=tk.BOTH, expand=True, padx=10, pady=10)
        right_panel = tk.Frame(self.plotting_tab, bg="#f0f0f0", width=600)
        right_panel.pack(side=tk.RIGHT, fill=tk.BOTH, expand=True, padx=10, pady=10)
        # Left panel content
        left_title = tk.Label(
            left_panel, text="Plot Functions", font=("Arial", 16, "bold"), bg="#f0f0f0"
        )
        left_title.pack(pady=10)
        # Function input
        func_frame = tk.Frame(left_panel, bg="#f0f0f0")
        func_frame.pack(fill=tk.X, pady=10)
        func_label = tk.Label(
            func_frame, text="Function(s):", font=("Arial", 12), bg="#f0f0f0"
        )
        func_label.pack(anchor=tk.W, padx=5, pady=5)
        self.plot_func = tk.Entry(func_frame, font=("Arial", 12), width=40)
        self.plot_func.pack(fill=tk.X, padx=5, pady=5)
        self.plot_func.insert(0, "sin(x), cos(x), x**2/10")
        func_help = tk.Label(
            func_frame,
            text="Separate multiple functions with commas",
            font=("Arial", 10),
            bg="#f0f0f0",
            fg="#7f8c8d",
        )
        func_help.pack(anchor=tk.W, padx=5)
        # Range input
        range_frame = tk.Frame(left_panel, bg="#f0f0f0")
        range_frame.pack(fill=tk.X, pady=10)
        range_label = tk.Label(
            range_frame, text="X Range:", font=("Arial", 12), bg="#f0f0f0"
        )
        range_label.grid(row=0, column=0, padx=5, pady=5, sticky=tk.W)
        from_label = tk.Label(
            range_frame, text="From:", font=("Arial", 12), bg="#f0f0f0"
        )
        from_label.grid(row=0, column=1, padx=5, pady=5)
        self.x_min = tk.Entry(range_frame, font=("Arial", 12), width=10)
        self.x_min.grid(row=0, column=2, padx=5, pady=5)
        self.x_min.insert(0, "-10")
        to_label = tk.Label(range_frame, text="To:", font=("Arial", 12), bg="#f0f0f0")
        to_label.grid(row=0, column=3, padx=5, pady=5)
        self.x_max = tk.Entry(range_frame, font=("Arial", 12), width=10)
        self.x_max.grid(row=0, column=4, padx=5, pady=5)
        self.x_max.insert(0, "10")
        # Plot title
        title_frame = tk.Frame(left_panel, bg="#f0f0f0")
        title_frame.pack(fill=tk.X, pady=10)
        title_label = tk.Label(
            title_frame, text="Plot Title:", font=("Arial", 12), bg="#f0f0f0"
        )
        title_label.pack(side=tk.LEFT, padx=5)
        self.plot_title = tk.Entry(title_frame, font=("Arial", 12), width=30)
        self.plot_title.pack(side=tk.LEFT, padx=5, fill=tk.X, expand=True)
        self.plot_title.insert(0, "Function Plot")
        # Plot button
        plot_btn = tk.Button(
            left_panel,
            text="Generate Plot",
            font=("Arial", 14),
            bg="#1abc9c",
            fg="white",
            padx=20,
            pady=10,
            command=self.plot_function,
        )
        plot_btn.pack(pady=20)
        # Right panel - plot area
        right_title = tk.Label(
            right_panel, text="Plot Output", font=("Arial", 16, "bold"), bg="#f0f0f0"
        )
        right_title.pack(pady=10)
        self.plot_frame = tk.Frame(right_panel, bg="white", bd=2, relief=tk.SUNKEN)
        self.plot_frame.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)
    def setup_matrix_tab(self):
        # Create main frame
        main_frame = tk.Frame(self.matrix_tab, bg="#f0f0f0")
        main_frame.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)
        title = tk.Label(
            main_frame,
            text="Matrix Operations",
            font=("Arial", 16, "bold"),
            bg="#f0f0f0",
        )
        title.grid(row=0, column=0, columnspan=3, pady=10)
        # Matrix A input area
        matrix_a_frame = tk.LabelFrame(
            main_frame,
            text="Matrix A",
            font=("Arial", 12),
            bg="#f0f0f0",
            padx=10,
            pady=10,
        )
        matrix_a_frame.grid(row=1, column=0, padx=10, pady=10, sticky="nsew")
        # Matrix dimensions
        dim_frame_a = tk.Frame(matrix_a_frame, bg="#f0f0f0")
        dim_frame_a.pack(fill=tk.X, pady=5)
        row_label_a = tk.Label(
            dim_frame_a, text="Rows:", font=("Arial", 12), bg="#f0f0f0"
        )
        row_label_a.pack(side=tk.LEFT, padx=5)
        self.rows_a = ttk.Spinbox(
            dim_frame_a, from_=1, to=10, width=5, font=("Arial", 12)
        )
        self.rows_a.pack(side=tk.LEFT, padx=5)
        self.rows_a.set(3)
        col_label_a = tk.Label(
            dim_frame_a, text="Columns:", font=("Arial", 12), bg="#f0f0f0"
        )
        col_label_a.pack(side=tk.LEFT, padx=5)
        self.cols_a = ttk.Spinbox(
            dim_frame_a, from_=1, to=10, width=5, font=("Arial", 12)
        )
        self.cols_a.pack(side=tk.LEFT, padx=5)
        self.cols_a.set(3)
        create_btn_a = tk.Button(
            dim_frame_a,
            text="Create Matrix",
            font=("Arial", 10),
            bg="#3498db",
            fg="white",
            padx=5,
            pady=2,
            command=lambda: self.create_matrix_inputs("A"),
        )
        create_btn_a.pack(side=tk.LEFT, padx=10)
        # Matrix entries frame
        self.matrix_a_entries_frame = tk.Frame(matrix_a_frame, bg="#f0f0f0")
        self.matrix_a_entries_frame.pack(fill=tk.BOTH, expand=True, pady=10)
        # Matrix B input area
        matrix_b_frame = tk.LabelFrame(
            main_frame,
            text="Matrix B",
            font=("Arial", 12),
            bg="#f0f0f0",
            padx=10,
            pady=10,
        )
        matrix_b_frame.grid(row=1, column=1, padx=10, pady=10, sticky="nsew")
        # Matrix dimensions
        dim_frame_b = tk.Frame(matrix_b_frame, bg="#f0f0f0")
        dim_frame_b.pack(fill=tk.X, pady=5)
        row_label_b = tk.Label(
            dim_frame_b, text="Rows:", font=("Arial", 12), bg="#f0f0f0"
        )
        row_label_b.pack(side=tk.LEFT, padx=5)
        self.rows_b = ttk.Spinbox(
            dim_frame_b, from_=1, to=10, width=5, font=("Arial", 12)
        )
        self.rows_b.pack(side=tk.LEFT, padx=5)
        self.rows_b.set(3)
        col_label_b = tk.Label(
            dim_frame_b, text="Columns:", font=("Arial", 12), bg="#f0f0f0"
        )
        col_label_b.pack(side=tk.LEFT, padx=5)
        self.cols_b = ttk.Spinbox(
            dim_frame_b, from_=1, to=10, width=5, font=("Arial", 12)
        )
        self.cols_b.pack(side=tk.LEFT, padx=5)
        self.cols_b.set(3)
        create_btn_b = tk.Button(
            dim_frame_b,
            text="Create Matrix",
            font=("Arial", 10),
            bg="#3498db",
            fg="white",
            padx=5,
            pady=2,
            command=lambda: self.create_matrix_inputs("B"),
        )
        create_btn_b.pack(side=tk.LEFT, padx=10)
        # Matrix entries frame
        self.matrix_b_entries_frame = tk.Frame(matrix_b_frame, bg="#f0f0f0")
        self.matrix_b_entries_frame.pack(fill=tk.BOTH, expand=True, pady=10)
        # Result area
        result_frame = tk.LabelFrame(
            main_frame,
            text="Result Matrix",
            font=("Arial", 12),
            bg="#f0f0f0",
            padx=10,
            pady=10,
        )
        result_frame.grid(row=1, column=2, padx=10, pady=10, sticky="nsew")
        self.matrix_result_frame = tk.Frame(result_frame, bg="#f0f0f0")
        self.matrix_result_frame.pack(fill=tk.BOTH, expand=True, pady=10)
        # Operations buttons
        ops_frame = tk.Frame(main_frame, bg="#f0f0f0")
        ops_frame.grid(row=2, column=0, columnspan=3, pady=10)
        button_styles = {
            "font": ("Arial", 12),
            "bg": "#3498db",
            "fg": "white",
            "padx": 15,
            "pady": 5,
        }
        add_btn = tk.Button(
            ops_frame,
            text="A + B",
            command=lambda: self.matrix_operation("add"),
            **button_styles,
        )
        add_btn.grid(row=0, column=0, padx=5, pady=5)
        subtract_btn = tk.Button(
            ops_frame,
            text="A - B",
            command=lambda: self.matrix_operation("subtract"),
            **button_styles,
        )
        subtract_btn.grid(row=0, column=1, padx=5, pady=5)
        multiply_btn = tk.Button(
            ops_frame,
            text="A × B",
            command=lambda: self.matrix_operation("multiply"),
            **button_styles,
        )
        multiply_btn.grid(row=0, column=2, padx=5, pady=5)
        det_a_btn = tk.Button(
            ops_frame,
            text="det(A)",
            command=lambda: self.matrix_operation("det_a"),
            **button_styles,
        )
        det_a_btn.grid(row=0, column=3, padx=5, pady=5)
        det_b_btn = tk.Button(
            ops_frame,
            text="det(B)",
            command=lambda: self.matrix_operation("det_b"),
            **button_styles,
        )
        det_b_btn.grid(row=0, column=4, padx=5, pady=5)
        inv_a_btn = tk.Button(
            ops_frame,
            text="A⁻¹",
            command=lambda: self.matrix_operation("inverse_a"),
            **button_styles,
        )
        inv_a_btn.grid(row=1, column=0, padx=5, pady=5)
        inv_b_btn = tk.Button(
            ops_frame,
            text="B⁻¹",
            command=lambda: self.matrix_operation("inverse_b"),
            **button_styles,
        )
        inv_b_btn.grid(row=1, column=1, padx=5, pady=5)
        trans_a_btn = tk.Button(
            ops_frame,
            text="Aᵀ",
            command=lambda: self.matrix_operation("transpose_a"),
            **button_styles,
        )
        trans_a_btn.grid(row=1, column=2, padx=5, pady=5)
        trans_b_btn = tk.Button(
            ops_frame,
            text="Bᵀ",
            command=lambda: self.matrix_operation("transpose_b"),
            **button_styles,
        )
        trans_b_btn.grid(row=1, column=3, padx=5, pady=5)
        eigen_btn = tk.Button(
            ops_frame,
            text="Eigenvalues A",
            command=lambda: self.matrix_operation("eigen_a"),
            **button_styles,
        )
        eigen_btn.grid(row=1, column=4, padx=5, pady=5)
        # Configure the grid weights
        for i in range(3):
            main_frame.grid_columnconfigure(i, weight=1)
        main_frame.grid_rowconfigure(1, weight=1)
        # Create the initial matrix inputs
        self.create_matrix_inputs("A")
        self.create_matrix_inputs("B")
    # Basic calculator functions
    def add_to_expression(self, value):
        current = self.entry_var.get()
        self.entry_var.set(current + value)
    def clear(self, type_clear):
        if type_clear == "C":
            current = self.entry_var.get()
            if current:
                self.entry_var.set(current[:-1])
        else:  # AC
            self.entry_var.set("")
    def add_function(self, func):
        function_map = {
            "sin": "sin(",
            "cos": "cos(",
            "tan": "tan(",
            "log": "log10(",
            "ln": "log(",
            "√": "sqrt(",
            "!": "!",
        }
        current = self.entry_var.get()
        if func == "!":
            self.entry_var.set(current + function_map[func])
        else:
            self.entry_var.set(current + function_map[func])
    def add_constant(self, const):
        constant_map = {"π": "pi", "e": "e"}
        current = self.entry_var.get()
        self.entry_var.set(current + constant_map[const])
    def calculate(self):
        try:
            expression = self.entry_var.get()
            expression = expression.replace("^", "**")
            # Handle factorial
            if "!" in expression:
                expression = self.handle_factorial(expression)
            # Create a safe local environment for eval
            safe_dict = {
                "sin": math.sin,
                "cos": math.cos,
                "tan": math.tan,
                "sqrt": math.sqrt,
                "log10": math.log10,
                "log": math.log,
                "pi": math.pi,
                "e": math.e,
                "factorial": math.factorial,
            }
            result = eval(expression, {"__builtins__": {}}, safe_dict)
            # Update history
            self.history_text.config(state=tk.NORMAL)
            self.history_text.insert(tk.END, f"{expression} = {result}\n")
            self.history_text.see(tk.END)
            self.history_text.config(state=tk.DISABLED)
            # Show result
            self.entry_var.set(str(result))
        except Exception as e:
            messagebox.showerror("Error", f"Invalid expression: {str(e)}")
    def handle_factorial(self, expr):
        # Replace n! with factorial(n)
        pattern = r"(\d+)!"
        return re.sub(pattern, r"factorial(\1)", expr)
    # Algebra functions
    def get_sympy_symbols(self, var_string):
        var_list = [v.strip() for v in var_string.split(",")]
        return [symbols(v) for v in var_list]
    def solve_equation(self):
        try:
            expr_str = self.algebra_expr.get()
            var_str = self.algebra_var.get()
            # Convert to SymPy expression
            x = symbols(var_str)
            # Check if it's an equation (contains =)
            if "=" in expr_str:
                left, right = expr_str.split("=")
                expr = sp.sympify(left) - sp.sympify(right)
            else:
                expr = sp.sympify(expr_str)
            # Solve
            solution = solve(expr, x)
            # Display result
            self.algebra_result.delete("1.0", tk.END)
            self.algebra_result.insert(tk.END, f"Equation: {expr_str}\n\n")
            self.algebra_result.insert(tk.END, f"Solution:\n{solution}")
        except Exception as e:
            messagebox.showerror("Error", f"Error solving equation: {str(e)}")
    def simplify_expression(self):
        try:
            expr_str = self.algebra_expr.get()
            # Convert to SymPy expression
            expr = sp.sympify(expr_str)
            # Simplify
            simplified = simplify(expr)
            # Display result
            self.algebra_result.delete("1.0", tk.END)
            self.algebra_result.insert(tk.END, f"Original: {expr_str}\n\n")
            self.algebra_result.insert(tk.END, f"Simplified:\n{simplified}")
        except Exception as e:
            messagebox.showerror("Error", f"Error simplifying expression: {str(e)}")
    def factor_expression(self):
        try:
            expr_str = self.algebra_expr.get()
            # Convert to SymPy expression
            expr = sp.sympify(expr_str)
            # Factor
            factored = sp.factor(expr)
            # Display result
            self.algebra_result.delete("1.0", tk.END)
            self.algebra_result.insert(tk.END, f"Original: {expr_str}\n\n")
            self.algebra_result.insert(tk.END, f"Factored:\n{factored}")
        except Exception as e:
            messagebox.showerror("Error", f"Error factoring expression: {str(e)}")
    def expand_expression(self):
        try:
            expr_str = self.algebra_expr.get()
            # Convert to SymPy expression
            expr = sp.sympify(expr_str)
            # Expand
            expanded = sp.expand(expr)
            # Display result
            self.algebra_result.delete("1.0", tk.END)
            self.algebra_result.insert(tk.END, f"Original: {expr_str}\n\n")
            self.algebra_result.insert(tk.END, f"Expanded:\n{expanded}")
        except Exception as e:
            messagebox.showerror("Error", f"Error expanding expression: {str(e)}")
    # Calculus functions
    def differentiate(self):
        try:
            expr_str = self.calculus_expr.get()
            var_str = self.calculus_var.get()
            # Convert to SymPy expression
            x = symbols(var_str)
            expr = sp.sympify(expr_str)
            # Differentiate
            derivative = diff(expr, x)
            # Display result
            self.calculus_result.delete("1.0", tk.END)
            self.calculus_result.insert(tk.END, f"Original: {expr_str}\n\n")
            self.calculus_result.insert(
                tk.END, f"Derivative with respect to {var_str}:\n{derivative}"
            )
        except Exception as e:
            messagebox.showerror("Error", f"Error differentiating: {str(e)}")
    def integrate(self):
        try:
            expr_str = self.calculus_expr.get()
            var_str = self.calculus_var.get()
            # Convert to SymPy expression
            x = symbols(var_str)
            expr = sp.sympify(expr_str)
            # Integrate
            integral = integrate(expr, x)
            # Display result
            self.calculus_result.delete("1.0", tk.END)
            self.calculus_result.insert(tk.END, f"Original: {expr_str}\n\n")
            self.calculus_result.insert(
                tk.END,
                f"Indefinite integral with respect to {var_str}:\n{integral} + C",
            )
        except Exception as e:
            messagebox.showerror("Error", f"Error integrating: {str(e)}")
    def definite_integral(self):
        try:
            expr_str = self.calculus_expr.get()
            var_str = self.calculus_var.get()
            lower = self.int_from.get()
            upper = self.int_to.get()
            # Convert to SymPy expression
            x = symbols(var_str)
            expr = sp.sympify(expr_str)
            # Convert limits
            lower_limit = sp.sympify(lower)
            upper_limit = sp.sympify(upper)
            # Integrate
            integral = integrate(expr, (x, lower_limit, upper_limit))
            # Display result
            self.calculus_result.delete("1.0", tk.END)
            self.calculus_result.insert(tk.END, f"Original: {expr_str}\n\n")
            self.calculus_result.insert(
                tk.END, f"Definite integral from {lower} to {upper}:\n{integral}"
            )
        except Exception as e:
            messagebox.showerror(
                "Error", f"Error calculating definite integral: {str(e)}"
            )
    def calculate_limit(self):
        try:
            expr_str = self.calculus_expr.get()
            var_str = self.calculus_var.get()
            point_str = self.limit_point.get()
            # Convert to SymPy expression
            x = symbols(var_str)
            expr = sp.sympify(expr_str)
            # Convert limit point
            if point_str.lower() == "inf" or point_str.lower() == "infinity":
                point = oo
            elif point_str.lower() == "-inf" or point_str.lower() == "-infinity":
                point = -oo
            else:
                point = sp.sympify(point_str)
            # Calculate limit
            lim = limit(expr, x, point)
            # Display result
            self.calculus_result.delete("1.0", tk.END)
            self.calculus_result.insert(tk.END, f"Original: {expr_str}\n\n")
            self.calculus_result.insert(
                tk.END, f"Limit as {var_str} approaches {point_str}:\n{lim}"
            )
        except Exception as e:
            messagebox.showerror("Error", f"Error calculating limit: {str(e)}")
    # Plotting functions
    def plot_function(self):
        try:
            func_str = self.plot_func.get()
            x_min = float(self.x_min.get())
            x_max = float(self.x_max.get())
            title = self.plot_title.get()
            # Clear previous plot
            for widget in self.plot_frame.winfo_children():
                widget.destroy()
            # Create new figure
            fig, ax = plt.subplots(figsize=(6, 4), dpi=100)
            fig.patch.set_facecolor("#f0f0f0")
            # Create x values
            x = np.linspace(x_min, x_max, 1000)
            # Split functions by comma
            functions = [f.strip() for f in func_str.split(",")]
            # Define safe functions for eval
            safe_dict = {
                "sin": np.sin,
                "cos": np.cos,
                "tan": np.tan,
                "sqrt": np.sqrt,
                "log": np.log,
                "log10": np.log10,
                "exp": np.exp,
                "pi": np.pi,
                "e": np.e,
                "abs": np.abs,
                "pow": np.power,
            }
            # Plot each function
            for func in functions:
                try:
                    # Replace ^ with ** for exponentiation
                    func = func.replace("^", "**")
                    # Evaluate function
                    y = eval(
                        f"[{func} for x in x]",
                        {"__builtins__": {}},
                        {**safe_dict, "x": x},
                    )
                    # Plot
                    ax.plot(x, y, label=func)
                except Exception as e:
                    messagebox.showwarning(
                        "Warning", f"Error plotting function '{func}': {str(e)}"
                    )
            # Configure plot
            ax.set_title(title)
            ax.set_xlabel("x")
            ax.set_ylabel("y")
            ax.grid(True)
            ax.axhline(y=0, color="k", linestyle="-", alpha=0.3)
            ax.axvline(x=0, color="k", linestyle="-", alpha=0.3)
            ax.legend()
            # Create canvas
            canvas = FigureCanvasTkAgg(fig, self.plot_frame)
            canvas.draw()
            canvas.get_tk_widget().pack(fill=tk.BOTH, expand=True)
        except Exception as e:
            messagebox.showerror("Error", f"Error plotting function: {str(e)}")
    # Matrix functions
    def create_matrix_inputs(self, matrix_id):
        if matrix_id == "A":
            rows = int(self.rows_a.get())
            cols = int(self.cols_a.get())
            parent_frame = self.matrix_a_entries_frame
        else:  # matrix_id == 'B'
            rows = int(self.rows_b.get())
            cols = int(self.cols_b.get())
            parent_frame = self.matrix_b_entries_frame
        # Clear previous entries
        for widget in parent_frame.winfo_children():
            widget.destroy()
        # Create matrix entries
        entries = []
        for i in range(rows):
            row_entries = []
            for j in range(cols):
                entry = tk.Entry(parent_frame, width=5, font=("Arial", 10))
                entry.grid(row=i, column=j, padx=2, pady=2)
                entry.insert(0, "0")
                row_entries.append(entry)
            entries.append(row_entries)
        # Store entries
        if matrix_id == "A":
            self.matrix_a_entries = entries
        else:
            self.matrix_b_entries = entries
    def get_matrix_values(self, matrix_id):
        if matrix_id == "A":
            entries = self.matrix_a_entries
        else:
            entries = self.matrix_b_entries
        rows = len(entries)
        cols = len(entries[0])
        # Extract values
        values = []
        for i in range(rows):
            row_values = []
            for j in range(cols):
                try:
                    value = float(entries[i][j].get())
                    row_values.append(value)
                except ValueError:
                    messagebox.showerror(
                        "Error",
                        f"Invalid value in Matrix {matrix_id} at position ({i+1},{j+1})",
                    )
                    return None
            values.append(row_values)
        return values
    def display_matrix_result(self, result, is_scalar=False):
        # Clear previous result
        for widget in self.matrix_result_frame.winfo_children():
            widget.destroy()
        if is_scalar:
            # Display scalar result
            result_label = tk.Label(
                self.matrix_result_frame,
                text=str(result),
                font=("Arial", 16),
                bg="#f0f0f0",
            )
            result_label.pack(expand=True)
        else:
            # Display matrix result
            rows = len(result)
            cols = len(result[0]) if rows > 0 else 0
            for i in range(rows):
                for j in range(cols):
                    val = result[i][j]
                    if isinstance(val, (int, float)):
                        # Format floating point numbers
                        if val == int(val):
                            val_str = str(int(val))
                        else:
                            val_str = f"{val:.4f}"
                    else:
                        val_str = str(val)

                    entry = tk.Entry(
                        self.matrix_result_frame, width=10, font=("Arial", 10)
                    )
                    entry.grid(row=i, column=j, padx=2, pady=2)
                    entry.insert(0, val_str)
                    entry.config(state="readonly", readonlybackground="white")
    def matrix_operation(self, operation):
        try:
            # Get matrices
            matrix_a_values = self.get_matrix_values("A")
            matrix_b_values = self.get_matrix_values("B")
            if matrix_a_values is None or (
                operation not in ["det_a", "inverse_a", "transpose_a", "eigen_a"]
                and matrix_b_values is None
            ):
                return
            # Convert to NumPy arrays for operations
            A = np.array(matrix_a_values)
            if operation not in ["det_a", "inverse_a", "transpose_a", "eigen_a"]:
                B = np.array(matrix_b_values)
            # Perform operation
            if operation == "add":
                if A.shape != B.shape:
                    messagebox.showerror(
                        "Error", "Matrices must have the same dimensions for addition"
                    )
                    return
                result = A + B
            elif operation == "subtract":
                if A.shape != B.shape:
                    messagebox.showerror(
                        "Error",
                        "Matrices must have the same dimensions for subtraction",
                    )
                    return
                result = A - B
            elif operation == "multiply":
                if A.shape[1] != B.shape[0]:
                    messagebox.showerror(
                        "Error",
                        f"Matrix dimensions incompatible for multiplication: ({A.shape[0]}x{A.shape[1]}) and ({B.shape[0]}x{B.shape[1]})",
                    )
                    return
                result = np.matmul(A, B)
            elif operation == "det_a":
                if A.shape[0] != A.shape[1]:
                    messagebox.showerror(
                        "Error", "Matrix must be square to calculate determinant"
                    )
                    return
                det = np.linalg.det(A)
                self.display_matrix_result(round(det, 6), is_scalar=True)
                return
            elif operation == "det_b":
                if B.shape[0] != B.shape[1]:
                    messagebox.showerror(
                        "Error", "Matrix must be square to calculate determinant"
                    )
                    return
                det = np.linalg.det(B)
                self.display_matrix_result(round(det, 6), is_scalar=True)
                return
            elif operation == "inverse_a":
                if A.shape[0] != A.shape[1]:
                    messagebox.showerror(
                        "Error", "Matrix must be square to calculate inverse"
                    )
                    return
                try:
                    result = np.linalg.inv(A)
                except np.linalg.LinAlgError:
                    messagebox.showerror(
                        "Error", "Matrix is singular, cannot compute inverse"
                    )
                    return
            elif operation == "inverse_b":
                if B.shape[0] != B.shape[1]:
                    messagebox.showerror(
                        "Error", "Matrix must be square to calculate inverse"
                    )
                    return
                try:
                    result = np.linalg.inv(B)
                except np.linalg.LinAlgError:
                    messagebox.showerror(
                        "Error", "Matrix is singular, cannot compute inverse"
                    )
                    return
            elif operation == "transpose_a":
                result = A.T
            elif operation == "transpose_b":
                result = B.T
            elif operation == "eigen_a":
                if A.shape[0] != A.shape[1]:
                    messagebox.showerror(
                        "Error", "Matrix must be square to calculate eigenvalues"
                    )
                    return
                try:
                    eigenvalues, eigenvectors = np.linalg.eig(A)
                    result_text = "Eigenvalues:\n"
                    for i, val in enumerate(eigenvalues):
                        result_text += f"λ{i+1} = {val.real:.4f}"
                        if val.imag != 0:
                            result_text += f" + {val.imag:.4f}i"
                        result_text += "\n"
                    result_label = tk.Label(
                        self.matrix_result_frame,
                        text=result_text,
                        font=("Arial", 12),
                        bg="#f0f0f0",
                        justify=tk.LEFT,
                    )
                    result_label.pack(expand=True, anchor=tk.W)
                    return
                except np.linalg.LinAlgError:
                    messagebox.showerror("Error", "Could not compute eigenvalues")
                    return
            # Display result
            self.display_matrix_result(result.tolist())
        except Exception as e:
            messagebox.showerror("Error", f"Error in matrix operation: {str(e)}")
if __name__ == "__main__":
    app = MathCalculator()
    app.mainloop()
