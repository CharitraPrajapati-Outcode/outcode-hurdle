# To develop a checklist for standard programming practices

## Description

Creating a checklist for standard programming techniques can help guarantee code quality, maintainability, and consistency between projects, which will eventually result in more dependable and efficient software development.

## Key Activities

- Involve research and compile best coding practices
- Develop and document the checklist and revise with team
- Validate with engineering team 

## Checklist

**Code Quality & Readability Checklist**

- [ ]  Meaningful Variable and Function Names.
    - Use descriptive, self-explanatory names for variables, functions, and classes (e.g., calculateTax() instead of calc())
- [ ]  Single Responsibility Principle (SRP)
    - Each function and class should do one thing and one thing only. Avoid bloated, multi-purpose methods.
- [ ]  Keep Functions Short and Focused
    - Functions should be concise and focused on a single task. If a function is too long, consider breaking it down into smaller functions.
- [ ]  Avoid Magic Numbers and Strings
    - Use constants or enums instead of hardcoding numbers and strings in your code. (e.g., const MAX_RETRIES = 3;)
- [ ]  DRY Principle (Don’t Repeat Yourself)
    - Avoid duplicating code by extracting common functionality into functions or classes.
- [ ]  Consistent Code Formatting
    - Use consistent indentation, spacing, and naming conventions throughout your codebase. Consider using a linter or code formatter to enforce consistent formatting. (eg Prettier, ESLint)
- [ ]  Comment and Document Where Necessary
    - Comment only when needed, explaining why (not how) something is done, especially in complex logic.
- [ ]  Remove Dead Code
    - Eliminate unused variables, functions, and commented-out code to keep the codebase clean.
- [ ]  Follow Language-Specific Best Practices
    - Adhere to specific guidelines for the programming language you're using (e.g., PEP8 for Python, Airbnb style guide for JavaScript).


**Code Structure & Organization Checklist**

- [ ]  Modularize Design
    - Break down code into reusable modules, libraries, or packages. Keep files small and focused on specific functionality
- [ ]  Follow a Clear Directory Structure
    - Organize files logically (e.g., components/, services/, models/ in a frontend project or controllers/, views/, routes/ in backend).
- [ ]  Separation of Concerns
    - Keep business logic separate from UI, database, and presentation layers. Follow MVC or similar architectural patterns
- [ ]  Single Source of Truth
    - Ensure that data is centralized, minimizing the chances of duplication or inconsistencies
- [ ]  Environment-Specific Configurations
    Use configuration files to manage environment-specific settings (e.g., development, production), and avoid hardcoding sensitive values in code

**Code Security Checklist**

- [ ]  Input Validation
    - Validate and sanitize user inputs to prevent common security vulnerabilities like SQL injection, XSS, and CSRF attacks.
- [ ]  Use Parameterized Queries/Prepared Statements
    - Ensure that all database queries are parameterized to avoid SQL injection attacks
- [ ]  Escape Output for HTML/JavaScript
    - Escape any user-generated data that is rendered on the frontend to prevent XSS attacks
- [ ]  Sensitive Data Encryption
    - Encrypt sensitive data (e.g., passwords, personal information) in storage and transit. Avoid storing sensitive data in plain text
- [ ]  Use Strong Authentication
    - Use strong authentication methods, such as OAuth or JWT, and ensure proper session management
- [ ]  Avoid Secrets in Code
    - Never commit sensitive information (e.g., API keys, database credentials) in version control. Use environment variables or secret management tools
- [ ]  Keep Dependencies Updated
    - Regularly update third-party libraries and dependencies to avoid security vulnerabilities in outdated packages
- [ ]  Implement Proper Error Handling
    - Use generic error messages for users and log detailed errors securely. Avoid exposing stack traces or sensitive info in error messages
- [ ]  Security Testing Conducted
    - Perform security testing and audits, including automated vulnerability scans and manual code reviews for potential security risks

**Code Performance & Optimization Checklist**

- [ ]  Optimize Loops and Conditions
    - Avoid unnecessary loops, and use efficient data structures (e.g., hashmaps, sets) to optimize performance
- [ ]  Minimize External Calls
    - Reduce the number of external API/database calls in your code. Batch requests or use caching mechanisms where applicable
- [ ]  Avoid Premature Optimization
    - Focus on clarity first, and optimize bottlenecks only when performance becomes a proven issue
- [ ]  Use Asynchronous Code Where Applicable
    - For I/O-heavy tasks (e.g., API calls, file handling), use asynchronous programming (e.g., async/await, promises) to improve responsiveness
- [ ]  Memory Management
    - Be mindful of memory usage, avoid memory leaks, and free up unused resources
- [ ]  Lazy Loading/On-Demand Loading
    - Load resources or components only when needed to improve performance and reduce initial load times

**Testing Checklist**

- [ ]  Unit Tests Written
    - Write unit tests for all critical functions and components. Each test should verify a single piece of functionality
- [ ]  Automated Tests Integrated
    - Ensure that the testing framework (e.g., Jest, Mocha, JUnit) is integrated into the development workflow with CI/CD pipelines
- [ ]  Edge Cases Handled
    - Write tests that cover a range of edge cases, including invalid inputs, boundary conditions, and unexpected user behavior
- [ ]  Integration Testing
    - Test the interaction between different modules or services to ensure that they work together seamlessly
- [ ]  Code Coverage Monitored
    - Aim for high code coverage with your tests, but remember that quality matters more than quantity. Focus on covering critical paths
- [ ]  Continuous Testing in CI/CD
    - Ensure that all tests are automatically run on every commit or pull request, preventing regression issues

**Version Control & Collaboration Checklist**

- [ ]  Meaningful Commit Messages
    - Write clear and descriptive commit messages explaining the purpose of the change (e.g., "Fix login authentication bug" instead of "Fix bug")
- [ ]  Frequent Commits
    - Commit code frequently with small, incremental changes, making it easier to track progress and rollback if necessary
- [ ]  Use Feature Branches
    - Always work on feature branches (e.g., feature/login-auth) rather than committing directly to the main or master branch
- [ ]  Pull Requests Reviewed
    - Ensure all pull requests undergo code reviews by at least one other team member before merging
- [ ]  Avoid Large, Monolithic Pull Requests
    - Break down pull requests into small, logical units of work to make them easier to review and test
- [ ]  Resolve Conflicts Early
    - Regularly pull changes from the main branch into your feature branch to avoid large, complex merge conflicts later

**Documentation & Knowledge Sharing Checklist**

- [ ]  In-Line Documentation Where Needed
    - Document complex or non-obvious code, explaining why a particular solution was implemented
- [ ]  README Files Updated
    - Keep README files updated with relevant instructions for setting up and using the project
- [ ]  API Documentation Available
    - For APIs, ensure that clear, concise documentation is available for developers (e.g., using Swagger or Postman collections)
- [ ]  Architecture Diagrams Provided (If Complex)
    - Include architecture diagrams for understanding the system’s components and interactions, especially in complex systems
- [ ]  Changelog Maintained
    - Maintain a changelog summarizing the significant changes, fixes, or updates between releases
- [ ]  Knowledge Sharing in Team
    - Conduct code walkthroughs, pair programming, and knowledge-sharing sessions to keep the entire team up to speed

**Final Quality Assurance Checklist**

- [ ]  Codebase Reviewed for Consistency
    - Review the overall codebase for consistency in naming conventions, structure, and style
- [ ]  No Obvious Bugs or Issues
    - Ensure that all known bugs are addressed and logged issues are resolved
- [ ]  Documentation Complete
    - Verify that all necessary documentation is complete and accessible
- [ ]  Security Best Practices Verified
    - Ensure that the final code adheres to all security best practices, with no sensitive information hardcoded
- [ ]  Deployment Ready
    - Confirm that the application is tested, optimized, and ready for deployment, with all necessary configurations in place

