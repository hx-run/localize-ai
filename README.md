# localize-ai

The `@kelesoglu/localize-ai` is an npm package that enables easy localization of your applications using OpenAI. It provides seamless integration with OpenAI's language models, allowing you to localize and adapt your applications to different languages and regions effortlessly
The kelesoglu/localize-ai library provides CI/CD support, allowing a separate pull request to be opened during the code review process specifically for translations. This enables the translations generated by the AI to be reviewed by third parties, ensuring their quality and accuracy.

## Localization: Making Your App Global

Localization is the process of adapting an application to be usable in different languages and regions. It involves adjusting the app's texts, dates, times, currency formats, and other local-specific features based on the user's language and regional preferences.

Localization has become increasingly important as apps need to cater to global markets and provide a personalized user experience. Users respond more positively to apps that offer an experience tailored to their language and culture.

The localization process can be time-consuming and challenging, especially for large and complex applications. Therefore, automating localization is a crucial step. Automation speeds up the translation process, reduces errors, and enables a more efficient localization workflow.

For more information, you can refer to [this Medium article](https://medium.com/@harunkeles0glu/best-practices-for-localization-in-microservices-2ad276481854).

## Installation

Install the library using npm:

```
npm install --save-dev @kelesoglu/localize-ai
```

## Usage

To use the **kelesoglu/localize-ai** library, follow these steps:

1. You should follow the directory structure below. languageCode.json file should be structured in a valid JSON format, where keys represent the source language strings, and values represent the translations in the respective language

```
locales/
  ├── en.json
  ├── tr.json
  ├── es.json
  └── fr.json
  └── ....
```

2. The library requires a configuration file (`localize-ai.config.js`) in the root directory of your project. Here's an example of the configuration file:

```javascript
module.exports = {
    localesDir: "./locales",  
    baseLanguage: "en",    // Replace it with any ISO 639-1 codes of your choice.
    targetLanguages: ['fr', 'es', 'de','tr'],
    apiKey: "YOUR_OPENAI_API_KEY"
}
```

3. Add a new script to run localize pre-commit:

```json
{
    "scripts": {
        "localize":"node -r @kelesoglu/localize"
    }
}
```

Make sure to replace `'YOUR_OPENAI_API_KEY'` with your actual OpenAI API key.

## CI/CD

The `kelesoglu/localize` library can be integrated with GitHub Actions and Bitbucket Pipelines for your CI/CD workflows.

### Configuration

`repository url` should be added in package.json as below
```json
{
  "name": "example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "localize":"node -r @kelesoglu/localize" // If you would like run localize in local
  },
  "repository": {
    "type": "git",
    "url": "git@github.com:your_organization/example.git" //https://github.com/your_organization/example.git
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@kelesoglu/localize": "^2.1.0",
  },
  "dependencies": {
    "husky": "^8.0.3"
  }
}
```

### GitHub Actions

To use `kelesoglu/localize-ai` with GitHub Actions, you need to add the following environment variables to your workflow YAML file:
```yml
name: YOUR_WORKFLOW_NAME

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    permissions:
      contents: write
      issues: write
      pull-requests: write
    runs-on: ubuntu-latest
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v3
        with:
          fetch-depth: 2
      
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: 18
          
      - name: Install dependencies
        run: npm install
        
      - name: Translate translations
        run: node -r @kelesoglu/localize-ai
        env: 
        # You need to create a token to be able to commit and pr
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
        # You need to type your openai api key as env variable
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}

```
Make sure to replace `'YOUR_WORKFLOW_NAME'` with the desired name for your workflow.
Please note that for Github Actions, you need to create a token and use it as the **GH_TOKEN** environment variable.

### Bitbucket Pipelines

To use kelesoglu/localize-ai with Bitbucket Pipelines, you need to add the following environment variable and configuration to your `bitbucket-pipelines.yml` file:

```yml
pipelines:
  branches:
    master:
      - step:
          name: Localize
          deployment: Production
          script:
            - npm install
            - node -r @kelesoglu/localize-ai
          env:
          # You need to create a token to be able to commit and pr
            BITBUCKET_TOKEN: $BITBUCKET_TOKEN
          # You need to type your openai api key as env variable
            OPENAI_API_KEY: $OPENAI_API_KEY

```
Please note that for Bitbucket Pipelines, you need to create a Bitbucket token and use it as the **BITBUCKET_TOKEN** environment variable.

## License

This library is licensed under the MIT License. See the [LICENSE](./LICENSE) file for more details.

## Contributing

Contributions are welcome! Please follow the [contribution guidelines](CONTRIBUTING.md) when making contributions to this project.

## Acknowledgements

This library is built upon various open-source packages and technologies. We would like to express our gratitude to the developers and contributors of those projects.

## Contact

If you have any questions or feedback, feel free to contact us at [harunkelesoglu_@hotmail.com](mailto:harunkelesoglu_@hotmail.com).

Enjoy using **localize-ai** and simplify your localization workflow with AI-powered translations!
