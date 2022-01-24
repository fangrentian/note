# 插入法

![](./images/sort-gif/insertion.gif)

```js
/**
 * @author: ares
 * @date: 2019/6/24 15:24
 * @description: 插入法,从数组中第二个值开始遍历,用当前遍历的值与当前值前面的所有值从后向前
 * 逐一比较,如果当前值小,则与比较的值交换位置,如果当前值大,则不交换位置
 * @param {array}array
 * @returns {string | array}
 */
function insertion(array) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	for (var i = 1; i < array.length; i++) {
		var currentValue = array[i],
			prevIndex = i - 1;
		while (prevIndex >= 0 && array[prevIndex] > currentValue) {
			array[prevIndex + 1] = array[prevIndex];
			prevIndex--;
		}
		array[prevIndex + 1] = currentValue;
	}
	return array;
};
```

# 冒泡法

![](./images/sort-gif/bubble.gif)

```js
/**
 * @author: ares
 * @date: 2019/6/24 15:39
 * @description: 冒泡法,从起始位置开始,两两比较,大值后移,循环操作,直到排序完成
 * 即大值后移
 * @param {array}array
 * @returns {string | array}
 */
function bubble(array) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	var len = array.length,
		temp;
	for (var i = 0; i < len; i++) {
		for (var j = 0; j < len - i - 1; j++) {
			if (array[j] > array[j + 1]) {
				temp = array[j + 1];
				array[j + 1] = array[j];
				array[j] = temp;
			}
		}
	}
	return array;
}
```

# 选择法

![](./images/sort-gif/selection.gif)

```js
/**
 * @author: ares
 * @date: 2019/6/24 16:13
 * @description: 选择法,从起始位置循环,查找数组中最小的值,将找到的最小值与当前遍历的值交换位置
 * 即小值前移
 * @param {array} array
 * @returns {string | array}
 */
function selection(array) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	var len = array.length,
		minValueIndex,
		temp;
	for (var i = 0; i < len - 1; i++) {
		minValueIndex = i;
		for (var j = i + 1; j < len; j++) {
			if (array[j] < array[minValueIndex]) minValueIndex = j;
		}
		temp = array[i];
		array[i] = array[minValueIndex];
		array[minValueIndex] = temp;
	}
	return array;
}
```

# 希尔法(缩小增量排序)

![](./images/sort-gif/shell.gif)

```js
/**
 * @author: ares
 * @date: 2019/7/14 8:51
 * @description: 希尔法(缩小增量排序),先按增量分组,再将分组按插入法排序
 * @param {array} array
 * @returns {*}
 */
function shell(array) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	var len = array.length;
	for (var gap = Math.floor(len / 2); gap > 0; gap = Math.floor(gap / 2)) {
		for (var i = gap; i < len; i++) {
			var j = i,
				current = arr[i];
			while (j - gap >= 0 && current < arr[j - gap]) {
				arr[j] = arr[j - gap];
				j = j - gap;
			}
			arr[j] = current;
		}
	}
	return array;
}
```

# 归并法

![](./images/sort-gif/merge.gif)

```js
/**
 * @author: ares
 * @date: 2019/7/14 10:51
 * @description: 归并法内部调用
 * @param {array} left  拆分的数组
 * @param {array} right 拆分的数组
 * @returns {Array}
 * @private
 */
function _merge(left, right) {
	var result = [];
	while (left.length > 0 && right.length > 0) left[0] <= right[0] ? result.push(left.shift()) : result.push(right.shift());
	while (left.length) result.push(left.shift());
	while (right.length) result.push(right.shift());
	return result;
}

/**
 * @author: ares
 * @date: 2019/7/14 10:49
 * @description: 归并法
 * @param {array} array
 * @returns {*}
 */
function merge(array) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	var len = array.length;
	if (len < 2) return array;
	var middle = Math.floor(len / 2),
		left = array.slice(0, middle),
		right = array.slice(middle);
	return _merge(merge(left), merge(right));
}
```

# 计数法

![](./images/sort-gif/counting.gif)

```js
/**
 * @author: ares
 * @date: 2019/7/14 11:48
 * @description: 计数法,用一个临时数组重新记录原数组,记录的方法是,用原数组中的值作为临时数组的索引,原数组中
 * 的值出现的次数作为临时数组对应索引的值.再遍历临时数组,重新填写原数组,适用于数组中的值不是很大并且序列比较集中时
 * @param {array} array
 * @param {number} maxValue  数组中的最大值
 * @returns {*}
 */
function counting(array, maxValue) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	var bucket = new Array(maxValue + 1),
		sortedIndex = 0,
		arrayLen = array.length,
		bucketLen = maxValue + 1;
	for (var i = 0; i < arrayLen; i++) {
		if (!bucket[array[i]]) bucket[array[i]] = 0;
		bucket[array[i]]++;
	}
	for (var j = 0; j < bucketLen; j++) {
		while (bucket[j] > 0) {
			array[sortedIndex++] = j;
			bucket[j]--;
		}
	}
	return array;
}
```

# 桶排序法

![](./images/sort-gif/bucket.gif)

```js
/**
 * @author: ares
 * @date: 2019/7/14 15:54
 * @description: 桶排序法,按数字中最大值和最小值将数组分块,用插入法排序分块,再重新填充原数组
 * @param{array}array
 * @param {unumber}bucketSize   桶数量
 * @returns {*}
 */
function bucket(array, bucketSize) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	var i,
		bucketSize = bucketSize || 5,
		minValue = array[0],
		maxValue = array[0];
	for (i = 1; i < array.length; i++) {
		if (array[i] < minValue) {
			minValue = array[i];
		} else if (array[i] > maxValue) {
			maxValue = array[i];
		}
	}
	var bucketCount = Math.ceil((maxValue - minValue) / bucketSize),
		buckets = new Array(bucketCount);
	for (i = 0; i < buckets.length; i++) {
		buckets[i] = [];
	}
	for (i = 0; i < array.length; i++) {
		buckets[Math.floor((array[i] - minValue) / bucketSize)].push(array[i]);
	}
	array.length = 0;
	for (i = 0; i < buckets.length; i++) {
		insertion(buckets[i]);
		for (var j = 0; j < buckets[i].length; j++) {
			array.push(buckets[i][j]);
		}
	}
	return array;
}
```

# 基数排序

![](./images/sort-gif/radix.gif)

```js
/**
 * @author: ares
 * @date: 2019/7/14 17:14
 * @description: 基数排序,从地位到高位分别分桶排序
 * @param {array} array
 * @param {number} maxDigit 最高位数
 * @returns {*}
 */
function radix(array, maxDigit) {
	var counter = [],
		mod = 10,
		dev = 1;
	for (var i = 0; i < maxDigit; i++, dev *= 10, mod *= 10) {
		for (var j = 0; j < array.length; j++) {
			var bucketIndex = parseInt((array[j] % mod) / dev);
			if (counter[bucketIndex] == null) counter[bucketIndex] = [];
			counter[bucketIndex].push(array[j]);
		}
		var pos = 0;
		for (var j = 0; j < counter.length; j++) {
			var value = null;
			if (counter[j] != null) {
				while ((value = counter[j].shift()) != null) array[pos++] = value;
			}
		}
	}
	return array;
}
```

# 快速排序

![](./images/sort-gif/quick.gif)

```js
/**
 * @author: ares
 * @date: 2019/7/14 18:02
 * @description: 快速排序
 * @param {array} array
 * @returns {*}
 */
function quick(array) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	var pivotIndex = Math.floor(array.length / 2),
		pivot = array.splice(pivotIndex, 1),
		left = [],
		right = [];
	for (var i = 0; i < array.length; i++) {
		if (array[i] < pivot[0]) {
			left.push(array[i]);
		} else {
			right.push(array[i]);
		}
	}
	return quick(left).concat(pivot, quick(right));
}
```

# 堆排序

![](./images/sort-gif/heap.gif)

```js
function _buildMaxHeap(array, len) {
	len = array.length;
	for (var i = Math.floor(len / 2); i >= 0; i--) {
		_heapify(array, i, len);
	}
}

function _heapify(array, i, len) {
	var left = 2 * i + 1,
		right = 2 * i + 2,
		largest = i;
	if (left < len && array[left] > array[largest]) largest = left;
	if (right < len && array[right] > array[largest]) largest = right;
	if (largest != i) {
		_swap(array, i, largest);
		_heapify(array, largest);
	}
}

/**
 * @author: ares
 * @date: 2019/7/14 11:25
 * @description: 交换数组中的两个值
 * @param {array} array
 * @param {number} i    索引值
 * @param {number} j    索引值
 * @private
 */
function _swap(array, i, j) {
	var temp = array[i];
	array[i] = array[j];
	array[j] = temp;
}

function heap(array) {
	if (Object.prototype.toString.call(array) !== '[object Array]') throw new TypeError('argument is not an Array!');
	if (array.length === 0) return array;
	var len = array.length;
	_buildMaxHeap(array, len);
	for (var i = array.length - 1; i > 0; i--) {
		_swap(array, 0, i);
		len--;
		_heapify(array, 0, len);
	}
	return array;
}
```