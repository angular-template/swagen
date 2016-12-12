# Swagen
CLI tool to generate client-side code based on Swagger definitions.

## Getting Started
> This section discusses installing the Swagen tool globally. We will also discuss installing it locally in your project to avoid global dependencies.

```sh
# Install Swagen CLI
npm install -g swagen

# Install a generator
npm install -g swagen-ng1-http

# Create configuration file (see below)

# Run swagen
swagen
```

In the root folder, create the Swagen configuration file (`swagen.config.js`):
```javascript
var petstore = {
    // Location of the Swagger JSON
    url: 'http://petstore.swagger.io/v2/swagger.json',
    
    // Location of the generated file
    output: './app/webservices/petstore.services.ts',
    
    // Generator and language to use
    generator: 'ng1-http',
    language: 'typescript',
    
    // Options to change identifiers in the generated code
    transforms: {
        serviceName: 'pascalCase',
        operationName: 'camelCase'
    },
    
    // Options specific to the generator package
    options: {
        moduleName: 'common',
        baseUrl: {
            provider: 'app.IConfig',
            variable: 'config',
            path: ['apiBaseUrl']
        },
        namespaces: {
            services: 'app.webservices',
            models: 'app.webservices'
        },
        references: [
            '../../../../typings/index.d.ts',
            '../../../../typings/app.d.ts'
        ]
};

module.exports = {
    petstore: petstore
};
```

### Further reading
[Installing Swagen locally in your project](https://github.com/angular-template/swagger-client/wiki/Installing-Swagen-locally-in-your-project)

## Generators
Swagen uses an ecosystem of generators to perform the actual code generation. Each generator is a Node package named `swagen-xxxx` where `xxxx` is the name of the generator. Examples are `swagen-ng1-http` or `swagen-dotnet`. Having a concept of generator packages allows 3rd-parties to write their own generators and plug them into the tool.

TBD: Writing a generator.

## Configuration
Swagen configuration is specified in a JavaScript file called `swagen.config.js`, typically located at the root of your project.
The `swagen.config.js` file exports a object literal, where each property represents one Swagger source that needs to be converted to code.

The available options for a single property are:
```javascript
var webService = {
    // URL location of the Swagger JSON definition.
    // Cannot be used with the file property
    url: 'https://my-webservice/swagger.json',
    
    // File location of the Swagger JSON definition
    // Cannot be used with the url property
    file: './data/webservices/swagger.json',
    
    // File to save the generated code in.
    output: './webservices/clients/my-webservice.ts',
    
    // Generator to use to generate the code.
    // Generator name is the same as the package name, without the starting 'swagen-'
    generator: 'ng2-http',
    
    // (Optional) Language to use for the generation
    // If the generator package supports more than one language, you will need to specify this.
    language: 'typescript',
    
    // (Optional) Transformation functions or pre-defined transformation names
    // Allows you to change identifiers defined in the Swagger JSON.
    transforms: {
        // (Optional) Name of the service
        serviceName: 'pascalCase',
        
        // (Optional) Name of the operation in a service
        operationName: (operationName, details) => {
            if (_.startsWith(operationName, 'Swagger_')) {
                return operationName.substr(8);
            }
            return operationName;
        },
        
        // (Optional) Name of the model
        modelName: (modelName, details) =>
            _.endsWith(modelName, 'Model') ? modelName.substr(0, modelName.length - 5) : modelName
    },
    
    // (Optional) Enable debug output
    debug: {
        // (Optional) Outputs the internal definition of the Swagger structure generated by Swagen
        definition: './.output/my-webservice.definition.json'
    },
    
    // Generator-specific options
    // See the generator's documentation for details.
    options: {
    }
}
```
TBD: Configuration file options
TBD: Writing configuration in JSON instead of JavaScript
TBD: Writing configuration in Typescript instead of JavaScript
