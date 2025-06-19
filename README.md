# ts-match

> TypeScript match.

## 安装

```bash
npm install @alibaba-inc/match
```

```typescript
function App() {
	let is_boolean = true;
	let is_boolean_rs = match<boolean, string>(is_boolean, {
		true: "is_boolean true pass",
		false: "is_boolean false pass",
	});

	let is_number = 0;
	let is_number_rs = match<number, string>(is_number, {
		0: "is_number 0 pass",
		_: "is_number other pass",
	});

	let is_string = "abc";
	let is_string_rs = match<string, string>(is_string, {
		abc: "is_string abc pass",
		_: "is_string other pass",
	});

	let return_fn = false;
	match(return_fn, {
		true: () => {},
		false: () => {
			return_fn = true;
		},
	});

	let condition = 6;
	let condition_rs = match<number, string>(condition, {
		"$ > 5 && $ < 10": "condition pass",
		"$ === 'a' || $ === 'b'": "condition pass",
		[`"$ === ${condition}"`]: "condition_rs is 6",
	});

	async function fetchData(): Promise<string> {
		return new Promise((resolve) => {
			setTimeout(() => {
				resolve("Data fetched successfully!");
			}, 1000);
		});
	}

	let [async_data, set_async_data] = useState("");

	useEffect(() => {
		(async () => {
			let fetch_data = match<string, Promise<string>>("test", {
				test: async () => {
					return await fetchData();
				},
				test2: () => {},
			});
			set_async_data(await fetch_data);
		})();
	}, []);

	return (
		<>
			<p>{is_boolean_rs}</p>
			<p>{is_number_rs}</p>
			<p>{is_string_rs}</p>
			<p>return_fn_rs: {return_fn ? "true" : "false"}</p>
			<p>{condition_rs}</p>
			<p>{async_data}</p>
			<p>
				{match("test", {
					test: <div style={{ color: "green" }}>test ReactNode</div>,
					test2: <div>test2 ReactNode</div>,
				})}
			</p>
			<p>
				{match("test2", {
					test: () => <div>test ReactNode</div>,
					test2: () => <div style={{ color: "blue" }}>test2 ReactNode</div>,
				})}
			</p>
		</>
	);
}
```
