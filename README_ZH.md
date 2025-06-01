# Equation Solver
## 方程求解器

一个用于求解一元多项式方程（1-4 次）的 Python 包，提供传统方法和 AI 辅助功能。

## 功能特性

```text
  - 求解 1 次到 4 次多项式方程
  - 使用本地语言模型进行 AI 辅助求解
  - 管理本地 AI 模型服务器
  - 表达式渲染和近似计算
  - 通过缓存提高性能
```

## 安装方法

```bash
pip install equation-solver
```

## 基本用法

### 传统求解方法

```python
from equation_solver import EquationSolver

solver = EquationSolver()
coefficients = solver.parse_input({
    "x⁴": "1",
    "x³": "0",
    "x²": "0",
    "x": "0",
    "常数项": "-1"
})

equation, degree = solver.build_equation(coefficients)

if degree > 0:
    solutions = solver.solve_equation(equation)
    processed = solver.process_solutions(solutions)
    print(f"找到 {len(processed)} 个解")
```

### AI 辅助求解方法

```python
from equation_solver import EquationSolver, AISolver, ModelServer

# 启动 AI 服务器
server = ModelServer()
server.start_server(model_path="path/to/model.gguf")

# 等待服务器启动
import time
while server.status != "running":
    time.sleep(1)

# 使用 AI 求解
solver = EquationSolver()
ai_solver = AISolver(port=5001)

coefficients = solver.parse_input({
    "x⁴": "1",
    "x³": "0",
    "x²": "0",
    "x": "0",
    "常数项": "-1"
})
equation, _ = solver.build_equation(coefficients)

equation_latex = sp.latex(equation)
ai_response = ai_solver.send_ai_request(equation_latex)
parsed = ai_solver.parse_ai_response(ai_response)

print("AI 解答：", parsed["clean_text"])
print("解：", parsed["solutions"])

# 停止服务器
server.stop_server()
```

## API 文档

### EquationSolver 类

- ```python`parse_input(inputs)```: 解析用户输入的系数
- ```python`build_equation(coefficients)```: 根据系数构建方程
- ```python`solve_equation(equation)```: 求解方程
- ```python`process_solutions(solutions)```: 准备用于显示的解
- ```python`get_cache_key(coefficients)```: 生成解的缓存键

### AISolver 类

- ```python`send_ai_request(equation_latex)```: 向 AI 服务器发送请求
- ```python`parse_ai_response(ai_text)```: 解析 AI 响应文本

### ModelServer 类

- ```python`start_server(model_path, port)```: 启动模型服务器
- ```python`stop_server()```: 停止服务器
- ```python`is_running()```: 检查服务器是否在运行
- ```python`get_logs(max_lines=100)```: 获取服务器日志

## 依赖项

```text
  - SymPy
  - Requests
  - Matplotlib
  - Pillow
  - Psutil
```

## 许可证

```text
MIT 许可证
```