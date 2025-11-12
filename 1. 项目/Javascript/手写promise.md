
```js
class MyPromise {
  constructor(executor) {
    this.status = 'pending';  // 状态：pending -> fulfilled / rejected
    this.value = null;        // 成功值
    this.reason = null;       // 失败原因
    this.onFulfilled = [];    // 成功回调队列
    this.onRejected = [];     // 失败回调队列

    const resolve = (value) => {
      if (this.status === 'pending') {
        this.status = 'fulfilled';
        this.value = value;
        this.onFulfilled.forEach(fn => fn(value));
      }
    };

    const reject = (reason) => {
      if (this.status === 'pending') {
        this.status = 'rejected';
        this.reason = reason;
        this.onRejected.forEach(fn => fn(reason));
      }
    };

    try {
      executor(resolve, reject);
    } catch (e) {
      reject(e);
    }
  }

  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      const fulfilled = (value) => {
        try {
          const result = onFulfilled ? onFulfilled(value) : value;
          resolve(result);
        } catch (err) {
          reject(err);
        }
      };

      const rejected = (reason) => {
        try {
          const result = onRejected ? onRejected(reason) : reason;
          reject(result);
        } catch (err) {
          reject(err);
        }
      };

      if (this.status === 'fulfilled') {
        fulfilled(this.value);
      } else if (this.status === 'rejected') {
        rejected(this.reason);
      } else {
        this.onFulfilled.push(fulfilled);
        this.onRejected.push(rejected);
      }
    });
  }
}

```