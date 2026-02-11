# 🛠️ Dynamic YAML Configurator for Hardware Simulations

A Django-powered tool designed for hardware engineers to manage complex simulation parameters through a user-friendly web interface.

## 💡 The Core Challenge: Data Reconstruction

When a nested YAML file is rendered in an HTML form, the structure becomes "flat." The biggest challenge is taking those flat key-value pairs (e.g., `tests.0.velocity`) and reconstructing them into a deeply nested Python Dictionary/YAML object.

---

## 🏗️ Technical Architecture

### 1. The "Unflattening" Engine

The backend features a custom **Unflattening Algorithm** that reconstructs data hierarchies using a separator-based logic (e.g., using `.` to denote a new level).

* **`unflatten_dict`**: Recursively builds nested dictionaries by splitting keys.
* **Recursive Detection**: It identifies parent-child relationships in the data to ensure the simulation engine receives the exact schema it expects.

### 2. Smart File Handling

* **Tkinter Integration**: Uses `filedialog` to allow engineers to pick configuration files directly from their local environment.
* **Safe Loading/Dumping**: Uses `PyYAML`'s `safe_load` and `safe_dump` (with `sort_keys=False`) to preserve the engineer's original file order, which is critical for readability in hardware testing.

### 3. Dynamic Rendering Workflow

1. **Load:** Python parses the `.yml` file into an `OrderedDict`.
2. **Render:** Django templates recursively walk the tree to build the UI.
3. **Submit:** The user edits values and adds new fields via JavaScript.
4. **Save:** The backend "unflattens" the POST request and overwrites the config file with the updated structure.

---

## 🛠️ Key Backend Functions

| Function | Description |
| --- | --- |
| `update_tests` | Orchestrates the file selection and initial render of the YAML tree. |
| `unflatten_dict` | The core logic that converts flat form keys back into nested dictionaries. |
| `get_dict_from_YML` | Recursively cleans and validates YAML data before it hits the frontend. |
| `save_yaml` | Ensures data integrity while writing back to the hardware simulation config. |

---

## 🚀 Impact for Hardware Engineers

* **Error Reduction:** No more syntax errors from missing indentation in YAML files.
* **Efficiency:** Quickly toggle simulation parameters without leaving the browser.
* **Accessibility:** Hardware specialists can focus on testing logic rather than file formatting.
To make your GitHub repository truly "plug-and-play" for recruiters or other engineers, a clear **How to Run** section is essential. It shows you think about the user experience and documentation.

Here is the "How to Run" section and a "Setup" guide you can add to the bottom of your README.

---

## 🚀 Getting Started

### 1. Prerequisites

* **Python 3.x**
* **Django**
* **PyYAML**
* **Tkinter** (Usually included with Python, used for the file picker)

```bash
pip install django pyyaml

```

### 2. Configuration (Environment Setup)

Currently, the project uses a hardcoded path for the simulation config. To run this on your machine, update the `file_path` in `views.py`:

```python
# Change this path to your local config location
FILE_PATH = 'path/to/your/example-config.yml' 

```

### 3. Running the Server

1. Clone the repository:
```bash[
git clone [https://github.com/sidrabilal71/Dynamic_YAML_Configurator]

```


2. Navigate to the project directory and run:
```bash
python manage.py runserver

```


3. Open `http://127.0.0.1:8000/update_tests/` in your browser.

---

## 🔄 Data Flow Visualization

Understanding how the data moves from a raw file to an editable UI and back is key to this project's value.

1. **Input:** The `load_yaml` function reads the hardware config.
2. **Transformation:** Data is passed through `get_dict_from_YML` to prepare it for the recursive template.
3. **Interaction:** The user interacts with `nested_field.html` to modify parameters.
4. **Reconstruction:** Upon saving, `unflatten_dict` processes the POST data to restore the original nested hierarchy.

---

## 📝 Future Roadmap

* [ ] Replace hardcoded paths with a `.env` configuration file.
* [ ] Add a "Reset" button to discard changes before saving.
* [ ] Implement a "Preview" mode to see the raw YAML output before writing to the file.

---
