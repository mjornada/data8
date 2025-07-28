# Data8_UFMS Development Guidelines

This document provides guidelines and instructions for developing and maintaining the Data8_UFMS project, which is a Portuguese translation of the UC Berkeley Data8 course materials.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Build and Configuration](#build-and-configuration)
3. [Testing Information](#testing-information)
4. [Development Guidelines](#development-guidelines)

## Project Overview

Data8_UFMS is a Portuguese translation of the UC Berkeley Data8 project (http://www.data8.org/), based on materials from the Spring 2022 version of the course (https://github.com/data-8/materials-sp22). The project consists of:

- **Book chapters**: Educational content in Jupyter notebooks and HTML format
- **Homework assignments**: Problem sets for students to complete
- **Labs**: Guided exercises for hands-on practice
- **Projects**: Larger assignments that integrate multiple concepts

## Build and Configuration

### Environment Setup

To work with this project, you need to set up a Python environment with the necessary dependencies:

1. Create a virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

2. Install the required dependencies:
   ```bash
   pip install jupyter numpy matplotlib
   pip install otter-grader
   pip install datascience
   ```

   Note: The `datascience` library is a custom library developed for the Data8 course. If it's not available on PyPI, you may need to install it from the Data8 GitHub repository.

### Converting Notebooks to HTML

The project contains both Jupyter notebooks (.ipynb) and HTML files. To convert notebooks to HTML:

1. Use nbconvert:
   ```bash
   jupyter nbconvert --to html path/to/notebook.ipynb
   ```

2. For batch conversion, you can use a script:
   ```bash
   for nb in $(find ./book -name "*.ipynb"); do
     jupyter nbconvert --to html "$nb"
   done
   ```

### Data Files

The notebooks reference data files in a relative path `../../assets/data/`. If these files are not present in your repository, you may need to:

1. Create the directory structure:
   ```bash
   mkdir -p assets/data
   ```

2. Download the data files from the original Data8 repository:
   ```bash
   # Example (adjust URLs as needed)
   wget -P assets/data/ https://www.inferentialthinking.com/data/top_movies_2017.csv
   ```

## Testing Information

The project uses multiple testing approaches depending on the context:

### 1. Otter Grader for Assignments

Homework assignments and labs use the [Otter Grader](https://otter-grader.readthedocs.io/) library for automated grading and testing.

#### How Otter Works in Notebooks:

1. Each notebook initializes otter at the beginning:
   ```python
   import otter
   grader = otter.Notebook("notebook_name.ipynb")
   ```

2. Tests are run using the `grader.check()` method:
   ```python
   grader.check("question_id")
   ```

3. At the end of the notebook, the submission is exported:
   ```python
   grader.export(pdf=False, run_tests=True)
   ```

#### Creating New Otter Tests:

To create new tests for assignments:

1. Define the expected output or behavior
2. Use the otter API to create test cases
3. Add the test to the notebook with appropriate question IDs

For detailed information, refer to the [Otter Grader documentation](https://otter-grader.readthedocs.io/).

### 2. Standard Python Testing

For testing Python modules outside of notebooks, use standard Python testing frameworks:

#### Example with unittest:

1. Create a module with functions to test:
   ```python
   # math_functions.py
   def calculate_mean(numbers):
       if not numbers:
           raise ValueError("Cannot calculate mean of an empty list")
       return sum(numbers) / len(numbers)
   ```

2. Create a test file:
   ```python
   # test_math_functions.py
   import unittest
   from math_functions import calculate_mean

   class TestMathFunctions(unittest.TestCase):
       def test_calculate_mean(self):
           self.assertEqual(calculate_mean([1, 2, 3, 4, 5]), 3.0)
           
       def test_calculate_mean_empty_list(self):
           with self.assertRaises(ValueError):
               calculate_mean([])

   if __name__ == '__main__':
       unittest.main()
   ```

3. Run the tests:
   ```bash
   python -m unittest test_math_functions.py
   ```

### 3. Custom Error Handling

The project includes a custom error handling system in `d8error.py` that provides user-friendly error messages for students. This system:

- Captures Python exceptions
- Provides helpful tips based on the error type (using errorConfig.json)
- Logs errors to a CSV file
- Collects feedback from users

When developing new content, ensure that the error handling system is properly integrated to provide a consistent experience for students.

## Development Guidelines

### Code Style

- Follow PEP 8 guidelines for Python code
- Use meaningful variable and function names
- Include docstrings for all functions and classes
- Add comments for complex code sections

### Notebook Structure

When creating or modifying notebooks:

1. Begin with a clear title and introduction
2. Include learning objectives
3. Structure content with appropriate headings
4. Include a mix of explanatory text, code examples, and exercises
5. End with summary points or review questions

### Translation Guidelines

When translating content from English to Portuguese:

1. Maintain the original structure and flow of the content
2. Adapt examples to be culturally relevant when appropriate
3. Ensure technical terms are translated consistently throughout the project
4. Preserve code functionality while translating comments and output messages

### Version Control

- Make atomic commits with clear commit messages
- Create feature branches for significant changes
- Use pull requests for code review before merging to main branches
- Keep the repository clean by not committing large data files or generated content

### Documentation

- Update README.md when making significant changes to the project structure
- Document any new dependencies or setup requirements
- Include comments in code to explain complex algorithms or non-obvious behavior
- Maintain this guidelines document with any new development practices

---

This document is intended to provide guidance for developers working on the Data8_UFMS project. For questions or clarifications, please contact the project maintainers.