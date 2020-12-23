# 🦊 ทำความรู้จัก useSelector\(\) และ useDispatch\(\) ใน react-redux Hooks

ถ้าจะพูดถึง react-redux version 7.1.0 ก็มีการ เพิ่ม ลบ และ เเก้ใขหลายอย่าง ถ้าอยากไป [ดูเพิ่ม](https://github.com/reduxjs/react-redux/releases/tag/v7.1.0)

โดย เมื่อ ก่อน ถ้าหากจะใช้งาน react-redux ก็จะต้องใช้งานผ่าน Higher Order Component.\(HOD\) `connect()` คลุม Component เราใว้ ****ใช่มั้ยครับ เเต่ version นี้ ไม่ต้อง สามารถใช้งานผ่าน function components. ได้เลย เหมือน Hooks ของ React เลย ในส่วนของ store ยังเหมือนเดิมนะครับ และจะใช้งานได้ก็ต่อเมื่ออยู่ภายใน Provider นะครับ!!

นั้นลองมาทำความรู้จัก useSelector\(\) และ useDispatch\(\) กัน

## `useSelector()` <a id="d4f8"></a>

คือ การดึง state ที่อยู่ใน redux store มา

## `useDispatch()` <a id="8c2e"></a>

คือ การเรียกใช้งาน dispatch

## `useStore()` <a id="4aeb"></a>

คือ การดึง Redux store ที่ใช้งาน อยู่นั้นเเละครับ ใว้ใช้ เข้าถึง ตัว reducers.

\*\*ทำมั้ยมันสั่นขนาดนี้ เเต่ อย่าลึมนะ มันสามารถเรียกใช้งานใน function components. ได้ โดยไม่ต้องผ่าน connect\(\) \*\*กล่าว 2 รอบเเล้ว เพราะ มัน ว้าววว!!

คงยังไม่เห็นภาพ นั้นลองดูการใช้งาน แบบเดิม และ เเบบใหม่นะครับ ขอขอบคุณ[ตัวอย่าง](https://dev.to/gsto/new-redux-hooks-a-before-and-after-comparison-are-they-better-loj)

* วิธีเดิม

![](https://miro.medium.com/max/1154/1*nNmKyKHNu-9vyLCIwSjkfA.png)

เราจะเห็นว่า มันก็คือการใช้ งาน react-redux เเบบเดิมๆ มีการใช้งานผ่าน HOD connect\(\) เเล้วมีการ ใช้ค่า ใน redux store คือ query เเละมีการ map dispatch 2 functions คือ updateQuery และ handleSearch โดยมี action ตามภาพนะครับ

```text
const connectedSearchBar = connect(  mapStateToProps,  mapDispatchToProps,)(defaultSearchBar)
```

* วิธีใหม่ Hooks พระเอกเรา

![](https://miro.medium.com/max/1192/1*FgwW-u2sFQHii6cwDHUwIA.png)

จากตัวอย่างด้านบน จะเห็นการใช้งาน useSelector และ useDispatch โดย เรียนใช้งานผ่าน function components. โดยมีการเรียกใช้งาน

```text
const query = useSelector(state => state.query)
```

เราก็สามารถ ใช้งาน query ที่อยู่ใน reducers. เราได้เเล้ว ว้าวว รอบเเรก

มาว้าวต่อกับการเรียกใช้งาน useDispatch ดังด้านล่างนี้

```text
const dispatch = useDispatch()
```

เราก็ สามารถใช้งาน dispatch ได้เเล้ว

เราอาจจะเเยกตัว action ออกมาเพื่อให้ Code ดู ง่ายขึ้นดังรูปด้านล่างนี้

![](https://miro.medium.com/max/1180/1*9Sfvd1LmwGYVRe-PSKNhSA.png)

code ก็จะดูดีขึ้น

```text
const { query, updateQuery, updateSearch } = useSearchQuery() //วิธีใช้งาน
```

อ้างอิง

* [https://react-redux.js.org/api/hooks](https://react-redux.js.org/api/hooks)
* [https://dev.to/gsto/new-redux-hooks-a-before-and-after-comparison-are-they-better-loj](https://dev.to/gsto/new-redux-hooks-a-before-and-after-comparison-are-they-better-loj)

