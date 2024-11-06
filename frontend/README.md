## SWR

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

## React query

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

## REFERENCES

https://swr.vercel.app/

https://react-query-v2.tanstack.com/overview

https://tanstack.com/query/v4/docs/overview
