# Careful Onboarding



<mark style="color:red;">**Don't: Do not call**</mark> <mark style="color:red;"></mark><mark style="color:red;">`onBoard`</mark> <mark style="color:red;"></mark><mark style="color:red;">**on a value that has not previously had**</mark> <mark style="color:red;"></mark><mark style="color:red;">`offBoard`</mark> <mark style="color:red;"></mark><mark style="color:red;">**called on it**</mark>

```solidity
contract BadContract {
  ctUint64 public sum;
  //...
  function addWithoutCarefulOnboard() public {
    gtUint64 a = MpcCore.setPublic64(1);
    gtUint64 sum_ = MpcCore.onBoard(sum); // THIS WILL REVERT
   
    sum_ = MpcCore.add(sum_, a);
    
    sum = MpcCore.offBoard(sum_);
  }
  //...
}
```

<mark style="color:blue;">**Do:**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">Initialize the value by calling</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`offBoard`</mark> <mark style="color:blue;"></mark><mark style="color:blue;">inside the constructor</mark>

```solidity
contract GoodContract {
  ctUint64 public sum;
  
  constructor() {
    // By initializing sum with an encrypted value, we ensure
    // calling onBoard on sum does not revert
    gtUint64 sum_ = MpcCore.setPublic64(0);
    sum = MpcCore.offBoard(sum_);
  }
  //...
  function add() public {
    gtUint64 a = MpcCore.setPublic64(1);
    gtUint64 sum_ = MpcCore.onBoard(sum);
   
    sum_ = MpcCore.add(sum_, a);
    
    sum = MpcCore.offBoard(sum_);
  }
  //...
}
```

<mark style="color:blue;">**Or:**</mark> <mark style="color:blue;">Check if the value is zero before calling</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`onBoard`</mark>

<pre class="language-solidity"><code class="lang-solidity"><strong>contract GoodContract {
</strong>  ctUint64 public sum;
  //...
  function addWithCarefulOnboard() public {
    gtUint64 a = MpcCore.setPublic64(1);
    gtUint64 sum_;
    
    // By checking if sum is equal to zero, we can verify whether
    // or not it is safe to call onBoard on sum
    if (ctUint64.unwrap(sum) == 0) {
        sum_ = MpcCore.setPublic64(0);
    } else {
        sum_ = MpcCore.onBoard(sum);
    }
   
    sum_ = MpcCore.add(sum_, a);
    
    sum = MpcCore.offBoard(sum_);
  }
  //...
}
</code></pre>
