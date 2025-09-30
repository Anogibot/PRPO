# Contributing to DX-LLaVA

We welcome contributions from the community! This document provides guidelines for contributing to DX-LLaVA.

## 🚀 Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/anonymous/DX-LLaVA.git
   cd DX-LLaVA
   ```
3. **Create a branch** for your feature or fix:
   ```bash
   git checkout -b feature/your-feature-name
   ```

## 📝 Development Guidelines

### Code Style

- Follow PEP 8 for Python code
- Use meaningful variable and function names
- Add docstrings to functions and classes
- Keep functions small and focused

### Code Formatting

We use `black` and `isort` for code formatting:

```bash
# Install development dependencies
pip install black isort flake8

# Format code
black llava/
isort llava/

# Check formatting
flake8 llava/
```

### Testing

- Add tests for new features
- Ensure all tests pass before submitting
- Run tests with: `python -m pytest tests/`

## 🔧 Types of Contributions

### Bug Reports

When filing a bug report, please include:
- **Environment**: Python version, CUDA version, GPU model
- **Steps to reproduce** the issue
- **Expected behavior** vs **actual behavior**
- **Error messages** or logs
- **Minimal code example** if applicable

### Feature Requests

For new features:
- **Describe the problem** you're trying to solve
- **Propose a solution** with implementation details
- **Consider backwards compatibility**
- **Provide examples** of the proposed API

### Code Contributions

1. **Discuss first**: Open an issue to discuss major changes
2. **Write tests**: Include tests for new functionality
3. **Document changes**: Update README and docs as needed
4. **Follow conventions**: Match existing code style and patterns

## 🔄 Pull Request Process

1. **Update documentation** if you changed APIs
2. **Add tests** for new functionality
3. **Ensure CI passes** (all tests, formatting checks)
4. **Write clear commit messages**:
   ```
   feat: add binary loss weighting for classification

   - Implement configurable loss weights for binary classification
   - Add validation for loss weight parameters
   - Update training documentation with new parameters
   ```

5. **Fill out the PR template** completely
6. **Request review** from maintainers

## 📋 Development Setup

### Environment

```bash
# Create development environment
conda create -n dx-llava-dev python=3.10 -y
conda activate dx-llava-dev

# Install in development mode
pip install -e ".[dev]"
```

### Pre-commit Hooks

We recommend using pre-commit hooks:

```bash
pip install pre-commit
pre-commit install
```

This will automatically format code and run checks before commits.

## 🏷️ Coding Standards

### Documentation

- Use Google-style docstrings:
  ```python
  def train_model(data_path: str, output_dir: str) -> None:
      """Train the DX-LLaVA model.

      Args:
          data_path: Path to training data JSON file
          output_dir: Directory to save model checkpoints

      Returns:
          None

      Raises:
          FileNotFoundError: If data_path doesn't exist
      """
  ```

### Error Handling

- Use specific exception types
- Provide helpful error messages
- Log errors appropriately

### Performance

- Profile code for performance bottlenecks
- Use appropriate data structures
- Consider memory usage for large models

## 🤝 Community Guidelines

- **Be respectful** and inclusive
- **Help others** learn and contribute
- **Give constructive feedback** in reviews
- **Credit others** for their work and ideas

## 📞 Getting Help

- **GitHub Issues**: For bugs and feature requests
- **GitHub Discussions**: For questions and general discussion
- **Code Review**: Tag maintainers for review

## 🏆 Recognition

Contributors will be recognized in:
- GitHub contributors list
- Release notes for significant contributions
- Project README acknowledgments

Thank you for contributing to DX-LLaVA! 🎉