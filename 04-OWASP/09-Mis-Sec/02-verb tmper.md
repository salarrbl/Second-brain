Verb tampering attacks **exploit either configuration flaws in the access control mechanism or vulnerabilities in the request handlers' code**. As presented in the example above, blocking requests that use non-standard HTTP methods is not enough because in many cases an attacker can use a legitimate HTTP method like HEAD.

## [yashar](https://www.youtube.com/watch?v=IRp0LurK23s&list=PPSV&t=20s) youtube  video
```bash
tamper() {
	echo $1
	for method in GET HEAD POST PUT DELETE; do
		echo $method `curl -s -k $1 $method -o /dev/null -w '%{http_code} - %{size_download}'`
	done
}
```


