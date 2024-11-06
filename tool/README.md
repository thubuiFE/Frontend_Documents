## Linters Format

### **Eslint**

Eslint là một mã nguồn mở, hỗ trợ cho việc tìm và tự động sửa lỗi code Javascript.

Install: `npm install eslint --save-dev` hoặc `yarn add eslint --dev`

Tạo _.eslintrc.json_: `npx eslint --init` hoặc `yarn run eslint --init`

File `.eslintrc.json` sẽ chứa các rule, đảm bảo viết code phải follow theo các rule này.
https://eslint.org/docs/latest/rules/

### **Prettier**

Prettier là một code formatter hỗ trợ cho: Javascript, JSX, Angular, Vue, Flow, Typescript, CSS, Less, SCSS, HTML,...

Install: `npm install --save-dev prettier` hoặc `yarn add --dev prettier`

Tạo file _.prettierrc.json_ để editor biết có sử dụng prettier, đây là file chứa các options prettier
https://prettier.io/docs/en/options.html

### **Lint-staged**

Lint-staged cho phép thực hiện một hoặc một số công việc chỉ đối với các file được `git staged`. Giúp tiết kiệm thời gian.

Install: `npm install --save-dev lint-staged` hoặc `yarn add --dev lint-staged`

Cấu hình lint-staged ở file `package.json`: thêm một số công việc cần thực hiện

    "lint-staged": {
        "\*.ts": [
        "npm run lint",
        "npm run format",
        "git add ."
        ]
    }

### **Husky**

Husky cho phép xử lý công việc trong các event của git như (add, commit,...) ngăn chặn kịp thời các lỗi không đáng có trước khi commit.

Install: `npm install --save-dev husky` hoặc `yarn add --dev husky`

Khởi tạo cấu hình: `npx husky install`. Folder `.husky` được tạo ra.

Bắt event `git commit`: `npx husky add .husky/pre-commit "yarn lint-staged"`. Nghĩa là khi bắt `git commit` sẽ chạy `yarn lint-staged`
