# Agent的提示词和指令文件（instructions.md）的建议使用方式

1. 在一开始就设定 5-10 条清晰的项目规则，让Agent了解你的结构和约束。
2. 在提示中要具体。详细说明技术栈、行为和约束。
3. 项目尽量要有合理的代码结构，并选择大小合适的任务，尽量在小块、集中的任务中生成代码、测试和审查。
4. 测试先行，确保在编写代码之前先编写测试用例。 
5. Agent工作的每一个步骤都要人的确认，Human in the loop，如果有错误及时修复，不要让错误积累。
6. 充分了解GH Agent的工作原理和上下文组成模式
7. 任务的背景和验收标准需最好可以包含在agent的上下文中。
8. 不要和Agent较劲，如果Agent陷入了某种困境，有时候切换成edits模式或者自己写，会更快。
9. 多用GH Copilot Chat的历史记录。有必要的时候，可以导入和导出。
10. 选择合适的模型，o系列，claude，gemini都有各自的优势和适用场景。


# .github\copilot-instructions.md 示例
默认会包含在chat/agent的上下文中
## 示例1:
```
# Copilot Instructions

This project is a web application that allows users to create and manage tasks. The application is built using React and Node.js, and it uses MongoDB as the database.

## Coding Standards

- Use camelCase for variable and function names.
- Use PascalCase for component names.
- Use single quotes for strings.
- Use 2 spaces for indentation.
- Use arrow functions for callbacks.
- Use async/await for asynchronous code.
- Use const for constants and let for variables that will be reassigned.
- Use destructuring for objects and arrays.
- Use template literals for strings that contain variables.
- Use the latest JavaScript features (ES6+) where possible.
```

## 示例2:
```
- If I tell you that you are wrong, think about whether or not you think that's true and respond with facts.
- Avoid apologizing or making conciliatory statements.
- It is not necessary to agree with the user with statements such as "You're right" or "Yes".
- Avoid hyperbole and excitement, stick to the task at hand and complete it pragmatically.

```

## 示例3:
```
This is a Go based repository with a Ruby client for certain API endpoints. It is primarily responsible for ingesting metered usage for GitHub and recording that usage. Please follow these guidelines when contributing:

## Code Standards

### Required Before Each Commit
- Run `make fmt` before committing any changes to ensure proper code formatting
- This will run gofmt on all Go files to maintain consistent style

### Development Flow
- Build: `make build`
- Test: `make test`
- Full CI check: `make ci` (includes build, fmt, lint, test)

## Repository Structure
- `cmd/`: Main service entry points and executables
- `internal/`: Logic related to interactions with other GitHub services
- `lib/`: Core Go packages for billing logic
- `admin/`: Admin interface components
- `config/`: Configuration files and templates
- `docs/`: Documentation
- `proto/`: Protocol buffer definitions. Run `make proto` after making updates here.
- `ruby/`: Ruby implementation components. Updates to this folder should include incrementing this version file using semantic versioning: `ruby/lib/billing-platform/version.rb`
- `testing/`: Test helpers and fixtures

## Key Guidelines
1. Follow Go best practices and idiomatic patterns
2. Maintain existing code structure and organization
3. Use dependency injection patterns where appropriate
4. Write unit tests for new functionality. Use table-driven unit tests when possible.
5. Document public APIs and complex logic. Suggest changes to the `docs/` folder when appropriate

```


# Prompts.md 示例
示例地址 .github/prompts/sample.prompt.md，以.prompt.md结尾 

## 示例1
```
# User Authentication

Our application will have `users` with the following information:

- The standard user information, including: `name`, `email` (unique).
- A link for a "magic" login, which will be a `GUID` as well as the expiration date for the link.
- Social login account IDs for Microsoft, GitHub, and Google.
- The timestamp of the last login.
- The timestamp for when the user was created
```

## 示例2
可以在一个prompt文件里引用另外一个prompt文件 
```
Generate a TypeScript interface for the table specified by the user. Refer to the [user schema](database_users.prompt.md).
``` 

## 示例3
可以在一个prompt文件里引用另外一个prompt文件
```
---
mode: 'agent'
tools: ['githubRepo', 'codebase']
description: 'Generate a new React form component'
---
Your goal is to generate a new React form component based on the templates in #githubRepo contoso/react-templates.

Ask for the form name and fields if not provided.

Requirements for the form:
* Use form design system components: [design-system/Form.md](../docs/design-system/Form.md)
* Use `react-hook-form` for form state management:
* Always define TypeScript types for your form data
* Prefer *uncontrolled* components using register
* Use `defaultValues` to prevent unnecessary rerenders
* Use `yup` for validation:
* Create reusable validation schemas in separate files
* Use TypeScript types to ensure type safety
* Customize UX-friendly validation rules

```