# 🦁 การใช้ OOP\(Abstract Factory\) ใน React



{% tabs %}
{% tab title="Main.js" %}
```javascript
import React, { useState } from 'react';
import { default as MyButton} from './MyStatus';


export default () => {
  const [status, setStatus] = useState('A');
  const myButton = new MyButton({status})
  return (
    <div>
      <h1>{status}</h1>
      <button onClick={()=>setStatus(status === 'A'? 'B': 'A')}>
        Change Status
      </button>
      <button onClick={myButton.handleClick1}>test 1</button>
      <button onClick={myButton.handleClick2}>test 2</button>
    </div>
  )
}
```
{% endtab %}

{% tab title="MyStatus.js" %}
```javascript
class Status {
  handleClick1() { throw new Error('No implementtion'); }
  handleClick2() { throw new Error('No implementtion'); }
}


class AStatus extends Status {
  handleClick1() {
    const callAPI = 'A1'
    console.log('is', callAPI)
  }
  handleClick2() {
    const callAPI = 'A2'
    console.log('is', callAPI)
  }
}

class BStatus extends Status {
  handleClick1() { 
    const callAPI = 'B1'
    console.log('is', callAPI)
  }
  handleClick2() {
    const callAPI = 'B2'
    console.log('is', callAPI)
  }
}

class AbstractFactoryMyStatus {
  constructor(props) {
    console.log('status', props)
    switch (props.status) {
      case 'A':
        return new AStatus();
      case 'B':
        return new BStatus();
      default:
        return new AStatus();
    }
  }
}
export default AbstractFactoryMyStatus;
```
{% endtab %}
{% endtabs %}



![Status A&#x200C;](https://gblobscdn.gitbook.com/assets%2F-MP8SO7JaQ7RmtwrvNjd%2F-MP8Xf_vnt9K7iJVco_D%2F-MP8ZGrmOeGEOdfJIq2O%2F1_RP9pBMiUafUAeow1W3OVnw.png?alt=media&token=1f72fe6e-ae98-43db-9148-4aba819ff369)

### **เมื่อ status เท่ากับ A**‌

* กดปุ่ม test 1 จะแสดง log "is A1**"**
* กดปุ่ม test 2 จะแสดง log "is A2"

![Status B&#x200C;](https://gblobscdn.gitbook.com/assets%2F-MP8SO7JaQ7RmtwrvNjd%2F-MP8Xf_vnt9K7iJVco_D%2F-MP8ZYEBJxmYEbOvYvay%2F1_ywPWt0N1JOm5OBe6Bbj7hg.png?alt=media&token=41d8f71f-cbb3-4dbe-b6d9-455ea53b02d5)

### **เมื่อ status เท่ากับ B**‌

* กดปุ่ม test 1 จะแสดง log "is B1"
* กดปุ่ม test 2 จะแสดง log "is B2"

