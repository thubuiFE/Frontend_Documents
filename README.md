# Frontend_Documents

## 1. What is CSR, SSR, SSG, ISR ?

### CSR

- **What**: CSR (Client Side Rendering), website được render trên web browser.
- **How**: Lúc đầu, browser sẽ load file HTML rỗng. Sau đó Javascript và styles sẽ được load sau
- **When**: Thường được sử dụng đối với những trang không quan trọng SEO.
- **Support**: React, Angular, Vue.

### SSR

- **What**: SSR (Server Side Rendering), website được render trên server, sau đó gửi cho client.
- **How**: Lúc đầu, gửi yêu cầu lên App Server, server sẽ render full HTML (bao gồm Javascript và styles) sau đó hiển thị ra cho client.
- **When**: Thường được sử dụng để tăng tốc độ và hỗ trợ SEO
- **Support**: React => NextJS, Angular => Angular Universal, Vue => NuxtJS.

### SSG

- **What**: SSR (Static Site Generation), website được render trong thời gian build. Host có thể là bất kì hosting tĩnh nào (netlify/vercel)
- **When**: Thường rất nhanh, performance tốt và hỗ trợ SEO. Thường được sử dụng khi build sales page, blog, các content tĩnh.
- **Support**: React => NextJS/Gatsby, Angular => Scully/Angular Universal, Vue => NuxtJS/Gridsome

### ISR

- **What**: ISR (Incremental Static Regeneration), website được làm mới trên fly based sau một thời gian được định nghĩa
- **When**: Tương tự như SSR, tuy nhiên không cần build sau mỗi lần thay đổi.
- **Support**: React => NextJS

---

## 2. Next.js Rendering Methods: CSR, SSR, SSG, ISR

### SSR

> `getServerSideProps`: Nextjs sẽ pre-rendered sau mỗi lần request và sử dụng data trả về như props thông qua `getServerSideProps`

- Chỉ chạy ở server-side
- Khi user truy cập vào page trực tiếp, getServerSideProps sẽ chạy để gọi request, page sẽ pre-rendered và trả về props.
- Khi user truy cập vào page bằng `next/link` hoặc `next/router`, Nextjs sẽ gửi API request đến server và chạy `getServerSideProps`
- Chỉ được export từ `pages`, phải được export như một funtion độc lập.

> `getInitialProps`: tương tự như `getServerSideProps` và `componentDidMount` lấy dữ liệu sau mỗi lần request.

- Là một async function
- Chỉ được sử dụng ở `pages`

### CSR

> `useSWR`

- Trước tiên, sẽ trả về dữ liệu cũ đã được lưu trong cache.
- Sau đó sẽ request để fetch data mới.
- Cập nhật data cuối cùng

### SSG

> `getStaticProps`: Nextjs sẽ pre-rendered tại thời điểm build và sử dụng data trả về như props thông qua `getStaticProps`

**When**

- Khi cần sử dụng data từ bên ngoài trong lúc build pages

**Note**

- Chỉ chạy ở server-side
- Chỉ được export từ `pages`, là một funtion độc lập

> `getStaticPaths`: Khi export `getStaticProps` sử dụng Dynamic Routes, Nextjs sẽ kiểm tra lại tất cả các path được khai báo trong `getStaticPaths`. `getStaticPath` sẽ trả về 1 object gồm có

- _Path_: trả về các paths params của page
- _Fallback_: `true`, `false`, `bloking`

**Note**

- `getStaticPaths` phải sử dụng với `getStaticProps`
- Chỉ được export từ `pages`, là một funtion độc lập

---

## 3. SWR

### **Introduce**

SWR (stale-while-revalidate) là thư viện React hook dùng để fetch data.

Cách hoạt động

- Lần đầu, trả về data đã lưu trong cache
- Sau đó gửi request
- Cập nhật lại data sau khi gọi request thành công

### **Hook useSWR**

useSWR nhận vào `key` và `fetcher` là asynchronous function

useSWR trả về 2 giá trị `data` và `error`

    import useSWR from 'swr'

    function Profile() {
        const { data, error } = useSWR('/api/user', fetcher)

        if (error) return <div>failed to load</div>
        if (!data) return <div>loading...</div>
        return <div>hello {data.name}!</div>
    }

---

## 4. React query

### **Introduce**

React query dùng để fetch, cache, update lại dữ liệu, xử lý các trạng thái loading,...

### **Cache data**

Khi tạo 1 query cần truyền vào

1. **queryKey**: là duy nhất, các query sẽ được phân biệt với nhau dựa vào query key
2. **queryFunction**: là một promise, dùng để xử lý data hoặc lỗi
3. **options**: https://tanstack.com/query/v4/docs/reference/useQuery?from=reactQueryV3&original=https://react-query-v3.tanstack.com/reference/useQuery

### **Handle loading, fetching**

isLoading là `true` khi api được gọi và cache rỗng.

isFetching là `true` khi cache có dữ liệu nhưng đang gọi queryFunction để update dữ liệu mới.

    const getListQuery = useQuery("list-query", callDataApi, {
        cacheTime: Infinity, //Thời gian cache data, ví dụ: 5000, sau 5s thì cache sẽ bị xóa, khi đó data trong cache sẽ là undefined
        refetchOnWindowFocus: false,
        staleTime: 10000,
    });

    const { data, isLoading, isFetching } = getListQuery;

---

## 5. Linters Format

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

## 5. REFERENCES

https://dev.to/pahanperera/visual-explanation-and-comparison-of-csr-ssr-ssg-and-isr-34ea

https://dev.to/mbaljeetsingh/what-is-csr-ssr-ssg-isr-different-rendering-strategies-and-which-framework-does-it-better-angular-react-vue-4lkp

https://nextjs.org/

https://swr.vercel.app/

https://react-query-v2.tanstack.com/overview

https://tanstack.com/query/v4/docs/overview
