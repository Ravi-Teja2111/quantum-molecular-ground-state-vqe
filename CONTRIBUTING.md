# Contributing to Quantum Simulation of Molecular Ground States

Thank you for your interest in contributing to this project! We welcome contributions from the community.

## How to Contribute

### Reporting Bugs

If you find a bug, please create an issue with:
- Clear description of the bug
- Steps to reproduce
- Expected vs actual behavior
- Environment details (Python version, Qiskit version, OS)
- Error messages or screenshots if applicable

### Suggesting Enhancements

For feature requests or improvements:
- Describe the enhancement clearly
- Explain the use case and benefits
- Provide examples if possible

### Pull Requests

1. **Fork the repository** and create your branch from `main`
2. **Write clear commit messages** describing your changes
3. **Add tests** if applicable
4. **Update documentation** for any changes in functionality
5. **Ensure code quality**:
   - Follow PEP 8 style guidelines
   - Add docstrings to functions/classes
   - Comment complex logic
6. **Submit the pull request** with a clear description

### Development Setup

```bash
# Clone your fork
git clone https://github.com/your-username/quantum-molecular-simulation.git
cd quantum-molecular-simulation

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install in development mode
pip install -e .
pip install -r requirements.txt

# Run tests (if available)
pytest tests/
```

### Code Style

- Follow [PEP 8](https://pep8.org/) guidelines
- Use meaningful variable and function names
- Keep functions focused and concise
- Add type hints where possible
- Write docstrings in NumPy/Google style

### Areas for Contribution

We especially welcome contributions in:

- **New molecular systems**: Add examples for different molecules
- **Ansatz improvements**: Implement new quantum circuit ansätze
- **Optimization algorithms**: Add or improve classical optimizers
- **Documentation**: Improve tutorials, examples, and explanations
- **Visualization**: Better plots and quantum circuit visualizations
- **Performance**: Optimize code for faster execution
- **Testing**: Add unit tests and integration tests
- **Error handling**: Improve robustness and error messages

### Questions?

Feel free to open an issue for any questions about contributing.

## Code of Conduct

Be respectful and inclusive. We aim to maintain a welcoming environment for all contributors.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
