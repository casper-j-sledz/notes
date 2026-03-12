> ## Angular, NodeJS, NPM installation
>> 1. Install [NodeJS](https://nodejs.org/en/download)
>> 2. Updating the CLI
>>    1. `npm uninstall -g angular-cli @angular/cli`
>>    1. `npm cache verify`
>>    1. `npm install -g @angular/cli@21.2.2`
>>    1. `npm install --save-dev @angular-devkit/build-angular`
>>    1. `npm install --save @angular-devkit/build-angular`
>>    1. `npm install --save --legacy-peer-deps`
>> 3) install bootstrap
>>    1. `npm install --save bootstrap@3`
>>    1. `angular.json` -> "styles" section  add ->  `node_modules\bootstrap\dist\css\bootstrap.min.css`
>> 4) Run app
>>    1. `ng new [app-name]`
>>    1. `ng`
>>    1. `npm start`
>>    1. `ng serve`
>>    1. `ng generate component [componentName]`
>>    1. `ng test --code-coverage`
>> ### Commands
>> | Description        | Command
>> | ------------------ | ---------------------------
>> | Cache verify       | `npm cache verify`
>> | Create new project | `ng new [appName]`
>> | Run dev server     | `ng serve` / `npm start` *if configured
>> | Generate service   | `ng g s [path\name]` default path is `src\app` eg: `ng g s modules\home\components\eg-name`
>> | Install package specified version | `npm install @angular/cli@21.2.2`
>> | Install package latest major version | `npm install @angular/cli@^21`
>> | Install package latest version | `npm install @angular/cli@latest`
>> | Install NgRx Store | `ng add @ngrx/store`
>> | Test with Code Coverage | `ng test --code-coverage`
>> | OpenSSL full check build | `Set-Variable NODE_OPTIONS=--openssl-legacy-provider; ng run [appName]:build:[environmentBuild]`
>> | Ignore package conflicts | `--legacy-peer-deps`
>> | Node Package Manager | `npm`
>> ### Software & VS Code plugins
>>> * [Visual Studio Code](https://code.visualstudio.com/download#)
>>>   * -> Add "Open with Code" to file      context menu
>>>   * -> Add "Open with Code" to directory context menu
>>>   * -> Extensions:
>>>     * -> Angular Snippets
>>>     * -> Code Spell Checker
>>>     * -> Debugger for Chrome
>>>     * -> ESLint
>>>     * -> Git History
>>>     * -> GitLens
>>>     * -> HTML Format
>>>     * -> JSON formatter
>>>     * -> JavaScript (ES6) code snippets
>>>     * -> Node.js Extension Pack
>>>     * -> Node.js Modules Intellisense
>>>     * -> -------------------------------------------
>>>     * -> Docker
>>>     * -> gitignore
>>>     * -> Bracket Pair Colorizer 2
>>>   * -> Browsers Debug Extensions:
>>>     * -> Chrome  -> DevTools for Chrome
>>>     * -> Edge    -> Microsoft Edge Tools for VS Code
>>>     * -> Firefox -> Debugger for Firefox
>>>   * -> Other Extensions:
>>>     * -> Angular Language Service
>>>     * -> Prettier - Code formatter
>>>     * -> TODO Highlight
>>>     * -> Todo Tree
>>> * [Git](https://git-scm.com/download/win)
>>>   * -> Use Visual Studio Code as Git's default editor
>>>   * -> Override the default brach name for repositories -> development
>>>   * -> Configure Git:
>>>     * -> git config user.name  "Name Surname"
>>>     * -> git config user.email "name.surname@domain.com"
>>> * [NodeJS](https://nodejs.org/en/download)
>>>   * -> Clone project:
>>>     * -> npm install
> ## Attach To Test Debug
>> `.vscode\launch.json`
>> ````json
>> {
>>   "version": "0.2.0",
>>   "configurations": [
>>     {
>>       "name": "Attach To Karma Tests",
>>       "type": "chrome",
>>       "request": "attach",
>>       "address": "localhost",
>>       "port": 9333,
>>       "pathMapping": {
>>           "/": "${workspaceRoot}/",
>>           "/base/": "${workspaceRoot}/"
>>       }
>>     }
>>   ]
>> }
>> ````
>> `karma.conf.js`
>> ````js
>> module.exports = function (config) {
>>   config.set({
>>     [...]
>>     // Make console output visible
>>     client: {
>>       [...]
>>       captureConsole: true,
>>       mocha: {
>>         bail: true,
>>       },
>>     },
>>     // Open port to attach to Karma Tests for VS Code Debugging
>>     port: 9876,
>>     browsers: ['ChromeDebugging'],
>>     customLaunchers: {
>>       ChromeDebugging: {
>>         base: 'Chrome',
>>         flags: ['--remote-debugging-port=9333'],
>>       },
>>     },
>>   });
>> };
> ## Code
>> ### Case Conversion
>>> * [Case Conversion](https://www.30secondsofcode.org/js/s/string-case-conversion)
>>> * `camelCase`
>>> * `PascalCase`
>>> * `kebab-case`
>>> * `snake_case`
>>> * `SCREAMING_SNAKE_CASE`
>>> * `Title Case`
>>> * `Sentence case`
>>> ````ts
>>> export function convertCase(str: string, separator: string = '_'): string | undefined {
>>>   return str && str.match(/[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g)?.join(separator);
>>> }
>> ### Conditional .http property binding
>>> `[pTooltip]="simulation.simulationStatus.name === 'Failed' ? tooltipContent : ''"`
>> ### Convert value to object property
>>> `const obj = { [this.valueName]: value }`
>> ### Dictionary
>>> ````ts
>>> export interface IExampleType {}
>>>
>>> export interface IExampleDictionary {
>>>   [key: string]: IExampleType;
>>> }
>>>
>>> export const exampleDictionary: IExampleDictionary = {
>>>   example1: {},
>>>   example2: {},
>>>   example3: {},
>>> };
>> ### ngClass
>>> * `[ngClass]="{ '<cssClass>': <booleanExpression> }"`
>>> * `[ngClass]="<booleanExpression> ? '<cssTrueClass>' : '<cssFalseClass>'"`
>> ### Output()
>>> `child.component.ts`
>>> ```ts
>>>   import { Output, EventEmitter } from '@angular/core';
>>>   export class ChildComponent {
>>>     @Output() stringEmitter: EventEmitter<string> = new EventEmitter<string>();
>>>     someMethod(): void { this.stringEmitter.emit("ExampleValue"); }
>>>   }
>>> ```
>>> `parent.component.ts`
>>> ```ts
>>>   export class AppComponent {
>>>     public myString: string;
>>>     onValueChange(value: string) { this.myString = value; }
>>>   }
>>> ```
>>> `parent.component.html`
>>> ```html
>>>  <app-child (stringEmitter)="onValueChange($event)"></app-child>
>> ### Select First Option On Enter Press
>>> `example.component.ts`
>>> ````ts
>>> export function selectFirstOption(autocomplete: MatAutocomplete): void {
>>>   autocomplete.options.first?.select();
>>> }
>>> ````
>>> `example.component.html`
>>> ````html
>>> <mat-form-field>
>>>     <input
>>>       matInput
>>>       [matAutocomplete]="autocompleteName"
>>>       (keyup.enter)="selectFirstOption(itemAutocomplete)">
>>>   <mat-autocomplete
>>>     #autocompleteName="matAutocomplete">
>>>     <mat-option>...</mat-option>
>>>   </mat-autocomplete>
>>> </mat-form-field>
>> ### Set Focus
>>> `example.component.ts`
>>> ````ts
>>> export class ExampleComponent {
>>>   @ViewChild('inputNameId') private inputNameRef: ElementRef;
>>>
>>>   public ngAfterViewInit(): void {
>>>     //delay focus to allow load of data
>>>     setTimeout(() => this.inputNameRef.nativeElement.focus(), 100);
>>>   }
>>>
>>>   public focus(): void {
>>>     this.inputNameRef.nativeElement.focus();
>>>   }
>>> }
>>> ````
>>> `example.component.html`
>>> ````html
>>> <input #inputNameId [...]>
> ## Docker local_network for Angular
>> ### default.conf
>> ```ts
>> server {
>>   // [...]
>>   location ~ ^/(partialURL)$ {
>>     set $target_host "http://service.name:80";
> ## Errors
>> ### An unhandled exception occurred: Cannot find module 'css'
>>> * **ERROR:** An unhandled exception occurred: Cannot find module 'css'
>>> * **Solution:**
>>>   1. `Remove-Item "node_modules" -Force -Recurse`
>>>   1. `npm ci`
>> ### 'tsc' is not recognized as internal or external command
>>> * **ERROR:** 'ng' is not recognized as internal or external command
>>> * **Solution:**
>>>   * `npm install -g @angular/cli@latest`
>>>   * if after cmd restart still fails add `%AppData%\npm\node_modules\@angular\cli\bin` to environment variables
>> ### 'tsc' is not recognized as internal or external command
>>> * **ERROR:** 'tsc' is not recognized as internal or external command
>>> * **Solution:** `npm install -g typescript`
>> ### npm: cannot be loaded because running scripts is disabled on this system.
>>> * **ERROR:** npm : File C:\Program Files\nodejs\npm.ps1 cannot be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
>>> * **Solution:** `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`
>> ### npm WARN config global `--global`, `--local` are deprecated. Use `--location=global` instead.
>>> * **ERROR:** npm WARN config global `--global`, `--local` are deprecated. Use `--location=global` instead.
>>> * **Solution:**
>>>     * `npm install -g npm-windows-upgrade`
>>>     * `npm-windows-upgrade`
>>>     * select version 8.13.2
>>>   * try
>>>     * `npm install -g npm@latest`
> ## Fixing vulnerabilities
>> * -> find error causing library -> crt + f:
>>   * Severity: critical
>> * -> go up to main library (in devDependencies)
>> * -> update it:
>>   * `npm update [mainLibrary]`
>>   * eg. `npm update @angular-devkit/build-angular`
>> * -> run audit to verify
>>   * `npm audit`
> ## HTML Attributes
>> * [CodeGuide.co](https://codeguide.co)
>> * class.
>> * id , name.
>> * data-*
>> * src , for , type , href , value.
>> * title , alt.
>> * role , aria-*
>> * tabindex.
>> * style.
> ## Jasmine test query params
>> ```ts
>> const queryCheck = (queryFragment: string) => ({
>>   /*
>>   * The asymmetricMatch function is required, and must return a boolean.
>>   */
>>   asymmetricMatch: (obj: any) => {
>>     const query = obj?.query
>>     return query && typeof query === 'string' && query.includes(queryFragment);
>>   },
>>
>>   /*
>>   * The jasmineToString method is used in the Jasmine pretty printer. Its
>>   * return value will be seen by the user in the message when a test fails.
>>   */
>>   jasmineToString: () => `Query check should contain: '${queryFragment}'`,
>> });
> ## JSON server
>> * `npm install json-server -D`
>> * `npx json-server [pathToFile.json] --routes [pathToRoutesFile.json] --port [portNumber]`
>> * ****PUT:** `[hostAddress]/jsonFile/id`
>> * **Body:**
>> ```json
>> {
>> 	 "parameter1": "value1",
>> 	 "parameter2": "value2"
>> }
>>
>> ```
>> `routesFile.json`
>> ```json
>> {
>> 	"/api/*": "/$1"
>> }
> ## Lunch Debug for Angular App
>> `.vscode\launch.json`
>> ```json
>> {
>>   "version": "0.2.0",
>>   "configurations": [
>>     {
>>       "name": "Launch Chrome Incognito",
>>       "type": "chrome",
>>       "request": "launch",
>>       "url": "http://localhost:${appPort}",
>>       "webRoot": "${workspaceFolder}",
>>       "userDataDir": false,                  // Clear user data
>>       "runtimeArgs": ["--incognito"]         // Use incognito mode
>>     },
>>     {
>>       "name": "Launch Edge Private",
>>       "type": "msedge",
>>       "runtimeArgs": [
>>            "--inprivate",                    // Use incognito mode
>>           "--disable-web-security",          // Disable CORS
>>           "--disable-site-isolation-trials", // Disable CORS
>>           "--user-data-dir=${workspaceFolder}/.vscode/edge-dev-profile"
>>       ]
>>     }
>>   ]
>> }
> ## NVM (Node Version Manager)
>> * `nvm -v`
>> * `nvm list`
>> * `nvm use XX.XX.X`
>> * `rm -rf node_modules`
> ## SASS
>> ### Align to parent
>>> ```scss
>>> .parent-class { position: relative; }
>>> .child-class { position: absolute; }
>> ### Align to previous
>>> ```scss
>>> .element { position: relative; }
>> ### Animation appear
>>> ```scss
>>> .appearing-element {
>>>   opacity: 0;
>>>   animation: appear-animation $duration forwards;
>>> }
>>> @keyframes appear-animation {
>>>       0% { opacity: 0; }
>>>   100% { opacity: 100; }
>>> }
>> ### Animation grow
>>> #### SCSS
>>> ```scss
>>> .element-to-grow { width: $init-width; }
>>> .growing-element { animation: grow-animation $duration forwards; }
>>> @keyframes grow-animation {
>>>       0% { width: $init-width;  }
>>>     100% { width: $final-width; }
>>> }
>>> ```
>>> #### HTML
>>> ```html
>>> <example-tag
>>>     class="element-to-grow"
>>>     [class.growing-element]="growingActivated">
>>> </example-tag>
>>> ```
>>> #### TS
>>> ```ts
>>> public growingActivated: boolean = false;
>> ### Animation hover background slide
>>> ```scss
>>> .element-class {
>>>   overflow: hidden;
>>>   &:before {
>>>     content: '';
>>>     background: lightgray;
>>>     display: block;
>>>     position: absolute;
>>>     top: -25%;
>>>     height: 150%;
>>>     left: -25%;
>>>     opacity: 1;
>>>     transition: 0s;
>>>   }
>>>   &:hover:before {
>>>     padding-left: 200%;
>>>     opacity: 0;
>>>     transition: all 1s;
>>>   }
>>> }
>> ### Animation left-slide
>>> ```scss
>>> .sliding-element {
>>>   right: -$slide-width;
>>>   animation: left-slide-animation $duration forwards;
>>> }
>>> @keyframes left-slide-animation {
>>>       0% { right: -$slide-width; }
>>>   100% { right: 0px; }
>>> }
>> ### Apply to first level of child
>>> ```scss
>>> .parent > .child
>> ### Center vertically / horizontally
>>> ```scss
>>> .parent-class {
>>>    display: flex;
>>> // Align parent content
>>>    align-items: center;     /* align vertical */
>>>    justify-content: center; /* align horizontal */
>>> }
>>> // Align child class
>>> .child-class {
>>>    margin: auto 0; /* align vertical */
>>>    margin:0  auto; /* align horizontal */
>>> }
>> ### Extend SASS class
>>> ```scss
>>> @extend .[class-name];
>> ### Styles propagation
>>> ```css
>>> ::ng-deep .class-name {...}
>> ### Style Property Binding
>>> [Angular Dynamic Styling](https://ultimatecourses.com/blog/using-ngstyle-in-angular-for-dynamic-styling)
>>> ```scss
>>> [style.display]="'inline-block'"
>> ### Window / screen height
>>> `100vh`
> ## Text length checking
>> `Text`
>> ```text
>> 00cdefgh 10cdefgh 20cdefgh 30cdefgh 40cdefgh 50cdefgh 60cdefgh 70cdefgh 80cdefgh 90cdefgh 1
>> 01cdefgh 11cdefgh 21cdefgh 31cdefgh 41cdefgh 51cdefgh 61cdefgh 71cdefgh 81cdefgh 91cdefgh 2
>> 02cdefgh 12cdefgh 22cdefgh 32cdefgh 42cdefgh 52cdefgh 62cdefgh 72cdefgh 82cdefgh 92cdefgh 3
>> 03cdefgh 13cdefgh 23cdefgh 33cdefgh 43cdefgh 53cdefgh 63cdefgh 73cdefgh 83cdefgh 93cdefgh 4
>> 04cdefgh 14cdefgh 24cdefgh 34cdefgh 44cdefgh 54cdefgh 64cdefgh 74cdefgh 84cdefgh 94cdefgh 5
>> 05cdefgh 15cdefgh 25cdefgh 35cdefgh 45cdefgh 55cdefgh 65cdefgh 75cdefgh 85cdefgh 95cdefgh 6
>> 06cdefgh 16cdefgh 26cdefgh 36cdefgh 46cdefgh 56cdefgh 66cdefgh 76cdefgh 86cdefgh 96cdefgh 7
>> 07cdefgh 17cdefgh 27cdefgh 37cdefgh 47cdefgh 57cdefgh 67cdefgh 77cdefgh 87cdefgh 97cdefgh 8
>> 08cdefgh 18cdefgh 28cdefgh 38cdefgh 48cdefgh 58cdefgh 68cdefgh 78cdefgh 88cdefgh 98cdefgh 9
>> 09cdefgh 19cdefgh 29cdefgh 39cdefgh 49cdefgh 59cdefgh 69cdefgh 79cdefgh 89cdefgh 99cdefgh 0
>> ```
>> `Numeric`
>> ```text
>> 123456789 123456789 123456789 123456789 123456789 123456789 123456789 123456789 123456789 1
> ## Tools, Code Formatting
>> ### [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
>>> `.eslintrc.json`
>>> ````json
>>>   {
>>>     "root": true,
>>>     "ignorePatterns": ["projects/**/*", "node_modules/**", "src/app/common/core/nSwag/nSwag.ts"],
>>>     "parser": "@typescript-eslint/parser",
>>>     "plugins": ["@angular-eslint", "@typescript-eslint", "import", "prettier", "rxjs", "unused-imports"],
>>>     "extends": ["plugin:@angular-eslint/recommended", "plugin:prettier/recommended", "prettier"],
>>>     "overrides": [
>>>       {
>>>         "files": ["*.ts"],
>>>         "parserOptions": {
>>>           "project": ["tsconfig.json", "e2e/tsconfig.json"],
>>>           "createDefaultProgram": true
>>>         },
>>>         "extends": ["plugin:@angular-eslint/template/process-inline-templates"],
>>>         "rules": {
>>>           "@angular-eslint/component-class-suffix": ["error"],
>>>           "@angular-eslint/component-max-inline-declarations": [
>>>             "error",
>>>             {
>>>               "template": 0
>>>             }
>>>           ],
>>>           "@angular-eslint/component-selector": [
>>>             "error",
>>>             {
>>>               "type": "element",
>>>               "prefix": "kebab-case"
>>>             }
>>>           ],
>>>           "@angular-eslint/contextual-lifecycle": "error", // non-fixable error, has to be fixed manually
>>>           "@angular-eslint/directive-class-suffix": ["error"],
>>>           "@angular-eslint/directive-selector": [
>>>             "error",
>>>             {
>>>               "type": "attribute",
>>>               "prefix": "camelCase"
>>>             }
>>>           ],
>>>           "@angular-eslint/no-conflicting-lifecycle": "error", // non-fixable error, has to be fixed manually
>>>           "@angular-eslint/no-empty-lifecycle-method": ["error"],
>>>           "@angular-eslint/no-forward-ref": "error",
>>>           "@angular-eslint/no-host-metadata-property": "error", // non-fixable error, has to be fixed manually
>>>           "@angular-eslint/no-input-rename": ["error"],
>>>           "@angular-eslint/no-lifecycle-call": "error", // non-fixable error, has to be fixed manually
>>>           "@angular-eslint/no-output-native": ["error"],
>>>           "@angular-eslint/no-output-on-prefix": ["error"],
>>>           "@angular-eslint/no-output-rename": ["error"],
>>>           "@angular-eslint/no-pipe-impure": "error",
>>>           "@angular-eslint/no-input-prefix": [
>>>             "error",
>>>             {
>>>               "prefixes": ["on"]
>>>             }
>>>           ], // non-fixable error, has to be fixed manually
>>>           "@angular-eslint/prefer-output-readonly": ["error"],
>>>           "@angular-eslint/sort-lifecycle-methods": ["error"], // non-fixable error, has to be fixed manually
>>>           "@angular-eslint/sort-ngmodule-metadata-arrays": ["error"],
>>>           "@angular-eslint/use-lifecycle-interface": ["error"],
>>>           "@typescript-eslint/explicit-function-return-type": ["error"],
>>>           "@typescript-eslint/explicit-member-accessibility": [
>>>             "error",
>>>             {
>>>               "accessibility": "explicit",
>>>               "ignoredMethodNames": ["specificMethod", "whateverMethod"],
>>>               "overrides": {
>>>                 "accessors": "off",
>>>                 "constructors": "no-public",
>>>                 "methods": "explicit",
>>>                 "properties": "explicit",
>>>                 "parameterProperties": "explicit"
>>>               }
>>>             }
>>>           ],
>>>           "@typescript-eslint/naming-convention": [
>>>             "error",
>>>             {
>>>               "selector": "function",
>>>               "format": ["PascalCase", "camelCase"]
>>>             },
>>>             {
>>>               "selector": "variable",
>>>               "format": ["camelCase", "UPPER_CASE", "PascalCase"]
>>>             }
>>>           ],
>>>           "@typescript-eslint/no-empty-function": "error",
>>>           "@typescript-eslint/no-unused-expressions": "off",
>>>           "@typescript-eslint/quotes": [
>>>             "error",
>>>             "single",
>>>             {
>>>               "avoidEscape": true
>>>             }
>>>           ],
>>>           "array-element-newline": [
>>>             "error",
>>>             {
>>>               "ArrayExpression": "consistent",
>>>               "ArrayPattern": {
>>>                 "minItems": 3
>>>               }
>>>             }
>>>           ],
>>>           "arrow-parens": ["error", "always"],
>>>           "import/no-deprecated": "error",
>>>           "import/order": "error",
>>>           "jsdoc/newline-after-description": "off",
>>>           "max-classes-per-file": "error",
>>>           "max-len": [
>>>             2,
>>>             {
>>>               "code": 120,
>>>               "ignorePattern": "^import\\W.*"
>>>             }
>>>           ],
>>>           "newline-per-chained-call": "error",
>>>           "no-empty": "error",
>>>           "no-multiple-empty-lines": "error",
>>>           "no-underscore-dangle": "off",
>>>           "prefer-arrow-callback": "off",
>>>           "prefer-arrow/prefer-arrow-functions": "off",
>>>           "quotes": "off",
>>>           "rxjs/no-exposed-subjects": "error",
>>>           "rxjs/no-unsafe-subject-next": "error",
>>>           "unused-imports/no-unused-imports": "error"
>>>         }
>>>       },
>>>       {
>>>         "files": ["*.html"],
>>>         "extends": ["plugin:@angular-eslint/template/recommended"]
>>>       }
>>>     ]
>>>   }
>> ### [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
>>> `.prettierrc`
>>> ````json
>>> {
>>>   "htmlWhitespaceSensitivity": "ignore",
>>>   "printWidth": 120,
>>>   "tabWidth": 2,
>>>   "useTabs": false,
>>>   "semi": true,
>>>   "singleQuote": true,
>>>   "trailingComma": "all",
>>>   "bracketSpacing": true,
>>>   "arrowParens": "always",
>>>   "proseWrap": "always",
>>>   "endOfLine": "auto",
>>>   "overrides": [
>>>     {
>>>       "files": ["*.html"],
>>>       "options": {
>>>         "parser": "html"
>>>       }
>>>     },
>>>     {
>>>       "files": ["*.component.html"],
>>>       "options": {
>>>         "parser": "angular"
>>>       }
>>>     },
>>>     {
>>>       "files": ["*.scss"],
>>>       "options": {
>>>         "singleQuote": false
>>>       }
>>>     }
>>>   ]
>>> }
> ## Unit Test
>> ### Test example
>>> ```ts
>>> import { MockComponent, MockModule } from 'ng-mocks';
>>> import { MsalModule } from '@azure/msal-angular';
>>> import { TestBed } from '@angular/core/testing';
>>>
>>> describe('AppComponent', () => {
>>>   let fixture: ComponentFixture<TestedComponent>;
>>>   let component: TestedComponent;
>>>   let initialData: T;
>>>
>>>   const exampleServiceSpy: Spy<ExampleServiceSpy> = createSpyFromClass(ExampleServiceSpy);
>>>   const mockObservable$ = new BehaviorSubject<T>(initialData);
>>>
>>>   beforeEach(() => {
>>>     TestBed.configureTestingModule({
>>>       declarations: [MockComponent(ExampleComponent)],
>>>       imports: [MsalModule, MockModule(ExampleModule)],
>>>       providers: [
>>>         MockProvider(ExampleProvider),
>>>         MockProvider(ExampleServiceSpy, exampleServiceSpy),
>>>         MockProvider(ExampleServiceObj, {
>>>           serviceProperty: [],
>>>         }),
>>>       ],
>>>     });
>>>
>>>     const exampleInject = TestBed.inject(ExampleInject);
>>>     spyOn(exampleInject.propertyObj, 'exampleMethod').and.returnValue('exampleValue');
>>>
>>>     const exampleObjectWithObservable = TestBed.inject(ExampleObjectWithObservable);
>>>     exampleObjectWithObservable.observable$ = jasmine.createSpy().and.returnValue(mockObservable$.asObservable());
>>>
>>>     fixture = TestBed.createComponent(TestedComponent);
>>>     component = fixture.componentInstance;
>>>     fixture.detectChanges();
>>>   });
>>>
>>>   it('ngOnInit should call exampleMethod with initialData', (): void => {
>>>     // Arrange
>>>     const spyMethod = spyOn(component, 'exampleMethod');
>>>
>>>     // Act
>>>     component.ngOnInit();
>>>
>>>     // Assert
>>>     expect(spyMethod).toHaveBeenCalledWith(initialData);
>>>   });
>>> });
>> ### Mock Observable
>>> ```ts
>>> import { ComponentFixture, fakeAsync, TestBed } from '@angular/core/testing';
>>> import { MockComponent, MockProvider } from 'ng-mocks';
>>> import { BehaviorSubject} from 'rxjs';
>>> import { RouterTestingModule } from '@angular/router/testing';
>>> import { HttpClientTestingModule } from '@angular/common/http/testing';
>>> import { MyComponent } from 'src/app/modules/my-skills/components/my-skills-filter/my-skills-filter.component';
>>> import { OtherComponent } from 'src/app/common/core/models/entities/user';
>>> import { MyService } from 'src/app/common/core/models/entities/category';
>>>
>>> describe('ExpertiseTabComponent', (): void => {
>>>   let component:   MyComponent;
>>>   let fixture:     ComponentFixture<MyComponent>;
>>>   let mockService: MyService;
>>>
>>>   const initialValue = 3;
>>>   const mockObservable$ = new BehaviorSubject<number>(initialValue);
>>>
>>>   beforeEach((): void => {
>>>     TestBed.configureTestingModule({
>>>       declarations: [MockComponent(OtherComponent)],
>>>       imports:      [RouterTestingModule, HttpClientTestingModule],
>>>       providers:    [MockProvider(MyService)],
>>>     }).compileComponents();
>>>
>>>     mockService = TestBed.inject(MyService);
>>>
>>>     mockService.MyObservable$ = jasmine.createSpy().and.returnValue(mockObservable$.asObservable());
>>>     mockService.CurrentValue$ = jasmine.createSpy().and.returnValue(mockObservable$.value);
>>>
>>>     fixture = TestBed.createComponent(MyComponent);
>>>     component = fixture.componentInstance;
>>>     fixture.detectChanges();
>>>   });
>>>
>>>   it('ngOnInit create object', (): void => {
>>>     // Arrange
>>>     const obj = new Object();
>>>
>>>     // Act
>>>     component.ngOnInit();
>>>
>>>     // Assert
>>>     expect(component.variable).toBeTruthy();
>>>     expect(component.variable).toBe(initialValue);
>>>     expect(component.variable).toEqual(obj);
>>>   });
>>>
>>>   it('ngOnInit should set areaOfExpertiseId', fakeAsync((): void => {
>>>     // Arrange
>>>     const mySpy = spyOn(component.contentUpdate, 'next');
>>>
>>>     component.areaOfExpertiseId$.subscribe((obj) => {
>>>       // Async Assert
>>>       expect(component.variable).toBeTruthy();
>>>       expect(component.variable).toBe(initialValue);
>>>       expect(component.variable).toEqual(obj);
>>>       expect(mySpy).toHaveBeenCalled();
>>>       expect(mySpy).toHaveBeenCalledTimes(1);
>>>       expect(mySpy).toHaveBeenCalledWith(arg1, arg2)
>>>     });
>>>
>>>     // Act
>>>     component.ngOnInit();
>>>   }));
>>> });
> ## [Udemi Angular - Maximilian Schwarzmüller](TODO)