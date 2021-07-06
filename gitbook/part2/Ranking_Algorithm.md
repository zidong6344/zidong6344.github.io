# 排序算法(Ranking_Algorithm)

### 1.选择排序

依次选出最大值，排在最前。

**示例：**

```C++
template<class T>
void mySort(T arr[], int num) {
	for (int i = 0; i < num; i++) {
		int max = i;
		for (int j = i + 1; j < num; j++) {
			if (arr[max] < arr[j])
				max = j;
		}
		if (max != i) {
			T temp = arr[i];
			arr[i] = arr[max];
			arr[max] = temp;
		}
	}
}
void test() {
	char charARR[] = "acdbef";
	int charARR_num = sizeof(charARR) / sizeof(charARR[0]);
	mySort(charARR, charARR_num);
}
```

