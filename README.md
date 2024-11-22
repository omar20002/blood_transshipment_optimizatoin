
# Blood Transshipment Optimization Project

## Overview

This project focuses on optimizing the transshipment of blood units between hospitals to minimize shortages and waste. It uses mathematical modeling and simulation techniques to determine the best transshipment strategies. The approach is inspired by the work of Maryam Dehghani, Babak Abbasi, and Fabricio Oliveira on proactive transshipment in the blood supply chain.

## Project Structure

The project includes the following components:

1. **Data Generation**:
    - The `sobel_random_d` function generates random demand scenarios for two hospitals using Sobol sequences, which ensure a uniform distribution of scenarios.

2. **Model Definition**:
    - The `model_` function defines the optimization model using the IBM CPLEX optimization library. It includes variables for orders, transshipments, and inventory levels, along with constraints to ensure realistic blood management policies.

3. **Simulation**:
    - A simulation loop runs the optimization model for multiple iterations, updating the inventory levels based on the transshipment decisions and new orders.

## Dependencies

The project requires the following Python libraries:
- `numpy`
- `scipy`
- `docplex`
- `random`
- `tqdm`

You can install these dependencies using pip:
```bash
pip install numpy scipy docplex tqdm
```

## Usage

1. **Generating Demand Scenarios**:
    - The function `sobel_random_d` generates demand scenarios for the hospitals.

    ```python
    D = sobel_random_d()
    ```

2. **Defining and Solving the Model**:
    - The function `model_` defines the optimization model and solves it for given initial blood inventories and demand scenarios.

    ```python
    B = {(i, m): random.randint(0, 20) for i in range(1, num_hospitals + 1) for m in range(1, num_shelf_life + 1)}
    model, X, Y = model_(B, D)
    ```

3. **Running the Simulation**:
    - The simulation loop runs the model for a specified number of iterations, updating inventories based on the transshipment and ordering decisions.

    ```python
    from tqdm import tqdm

    invs = {}
    ords = {}
    trans = {}

    for i in tqdm(range(10)):    
        model, X, Y = model_(B, sobel_random_d())
        
        invs[i+1] = B.copy()
        ords[i+1] = Y.copy()
        trans[i+1] = X.copy()
        
        for i in N:
            for j in N:
                for m in M:
                    if i != j:
                        B[i, m] -= int(X[i, j, m].solution_value)
                        B[j, m] += int(X[i, j, m].solution_value)

        for i in N:
            B[i, 1] = 0
            B[i, 2] = B[i, 3]
            B[i, 3] = int(Y[i].solution_value)
    ```

## Results

The results of each simulation iteration, including inventory levels, orders, and transshipments, are stored in dictionaries `invs`, `ords`, and `trans`, respectively. These can be analyzed to evaluate the effectiveness of the transshipment strategies.

## Contributing

Contributions to improve the model and its application are welcome. Please fork the repository and submit a pull request with your enhancements.

## License

This project is licensed under the MIT License.

## References

- Maryam Dehghani, Babak Abbasi, Fabricio Oliveira, Proactive transshipment in the blood supply chain: A stochastic programming approach, Omega, Volume 98, 2021, 102112, ISSN 0305-0483, [https://doi.org/10.1016/j.omega.2019.102112](https://www.sciencedirect.com/science/article/pii/S0305048318308284)
