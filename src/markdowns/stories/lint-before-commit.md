Git commit 之前 调用 ESLint

在提交代码前, 可以使用ESLint对代码进行lint操作.
一般可以使用自己写的shell脚本, 代码如下

```sh
#!/bin/sh

STAGED_FILES=$(git diff --cached --name-only --diff-filter=ACM | grep ".vue\{0,1\}$" | grep -v "node_modules")

if [[ "$STAGED_FILES" = "" ]]; then
  exit 0
fi

PASS=true

echo "\nValidating Javascript:\n"

# Check for eslint
which eslint &> /dev/null
if [[ "$?" == 1 ]]; then
  echo "\t\033[41mPlease install ESlint\033[0m"
  exit 1
fi

for FILE in $STAGED_FILES
do
  eslint "$FILE"

  if [[ "$?" == 0 ]]; then
    echo "\t\033[32mESLint Passed: $FILE\033[0m"
  else
    echo "\t\033[41mESLint Failed: $FILE\033[0m"
    PASS=false
  fi
done

echo "\nJavascript validation completed!\n"

if ! $PASS; then
  echo "\033[41mCOMMIT FAILED:\033[0m Your commit contains files that should pass ESLint but do not. Please fix the ESLint errors and try again.\n"
  exit 1
else
  echo "\033[42mCOMMIT SUCCEEDED\033[0m\n"
fi

exit $?
```


或者使用npm上现成的第三方工具, 例如 `yorkie` 或 `husky`. 
之后可以在 `package.json` 中写好对应阶段的操作
例如: 

```
  "lint-staged": {
    "*.{js,vue}": [
      "vue-cli-service lint",
      "git add"
    ]
  },
  "gitHooks": {
    "pre-commit": "lint-staged"
  },
```