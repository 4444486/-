

# 二维vector的创建填充方式：

## 在创建的时候一个个元素填入

```c++
	vector<vector<int>> matrix {
        {1, 1, 1, 1},
        {2, 2, 2, 2},
        {3, 3, 3, 3}
    };

```



## 使用push_back

```c++
	vector<vector<int>> matrix;

    for (int i=0; i<m; ++i) {
        vector<int> row;

        for (int j=0; j<n; ++j) {
            row.push_back(i+1);
        }

        matrix.push_back(row);
    };
```



## 使用resize

```c++
	// Init
    vector<vector<int>> matrix(m);

    for (int i=0; i<m; ++i) {
        matrix[i].resize(n, i+1);
    };


```



## 使用一维定义二维(缺点：只能填充特定的一种字符)

```c++
	// Init
    vector<vector<int>> matrix(m, vector<int>(n, 1));
```

