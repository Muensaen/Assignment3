# Assignment3
# Tree COIN ICO Contract
1)ติดตั้ง NodeJS โดยไปดูวิธีติดตั้งได้เลยที่ https://nodejs.org/en/
สำหรับ Linux ทำตามนี้จะไม่ต้อง sudo เพื่อติดตั้งโปรแกรมผ่าน npm
2) ติดตั้งตัว Ethereum จำลองและตัว client สำหรับทดสอบด้วยคำสั่ง
npm install -g ganache-cli
3) ติดตั้ง Truffle เฟรมเวิร์คสำหรับพัฒนาภาษา Solidity (ภาษาหลักที่ใช้เขียน smart contract) ดังนี้
npm install -g truffle
4) สร้างโฟลเดอร์ที่จะเก็บ smart contract และย้ายเข้าไปใช้งาน โดยชื่อเหรียญที่ผมจะออกในตัวอย่างนี้จะใช้ชื่อว่า TreeCoin ตัวย่อ Tree ล่ะกันจะได้ดูน่ารักๆ
mkdir Treecoin-ico && cd Treecoin-ico
5) สั่งให้ truffle สร้างไฟล์เริ่มต้นของโปรเจค
truffle init
เนื่องจากการเขียน smart contract นั้นยากและถ้าเราเริ่มเขียนเองทั้งหมดอาจจะเกิดข้อผิดพลาดได้ง่ายๆ เราก็เลยต้องหาโค้ดที่เขียนและทดสอบออกมาเป็นอย่างดีแล้วมาเป็นตัวตั้ง ในที่นี้ผมจะใช้ OpenZeppelin ที่ค่อนข้างดีกว่าเขียนเองหมดอย่างแน่นอน โดยติดตั้งดังนี้
npm init -y
npm install --save @openzeppelin/contracts@v2.5 web3
6) เขียน smart contract ของเหรียญก่อนเลย โดยสร้างไฟล์ชื่อ TreeCoin.sol ที่โฟลเดอร์ contracts แล้วใส่โค้ดดังนี้

เป็นอันว่าได้เหรียญแล้วโดยการสืบทอดคุณสมบัติของ Contract MintableToken ที่ OpenZeppelin เขียนเอาไว้นั้นเอง เอาจริงๆ แล้ว contract ก็เหมือน Class ในภาษาเชิงวัตถุอื่นๆ นั่นแหละ Contract MintableToken คือ Token ที่สามารถทำเหรียญขึ้นมาเพิ่มเติมมาเพิ่มเรื่อยๆ นั้นเอง จึงไม่ได้จำกัดจำนวนเหรียญเอาไว้
7) เขียน Contract เพื่อขายเหรียญนั่นเอง โดยการสร้างไฟล์ชื่อ TreeCrowdSale.sol ที่โฟลเดอร์ contracts แล้วใส่โค้ดดังนี้

Contract นี้ก็จะสืบทอดความสามารถของ Contract ต่างๆ ที่เกี่ยวของมาใส่ TreeCrowdSale ไม่ว่าจะเป็นการจำกัดเวลาซื้อขายจาก TimedCrowdsale, การจำกัดจำนวนเงินสูงสุดที่จะระดมทุนด้วย CappedCrowdsale, การคืนเงินถ้าระดมทุนไม่ถึงเป้าจาก RefundableCrowdsale รวมกันจนเป็นสิ่งที่เราต้องการได้ถ้าต้องการเพิ่มหรือตัดออกฟีเจอร์ออกก็ทำได้เลยจากตรงนี้
8) การสร้าง script สำหรับติดตั้ง smart contract เข้าไปใน Ethereum Blockchain นั่นเอง โดยการสร้างไฟล์ 2_deploy_contracts.js ในโฟล์เดอร์ migrations ดังนี้

ผมได้ตั้งไว้ว่าจะระดมทุนขั้นต่ำ 5,000 ETH และสูงสุด 10,000 ETH ด้วยราคาขาย 20,000 Token ต่อ 1 ETH เริ่มขายจากปัจจุบันไปจนถึง วันที่ 9  กันยายน 2021 เวลา 09:09:09 UTC ไฟล์นี้สำคัญมาก โดยมันจะไปสั่งสร้าง token ก่อนและก็สร้าง crowsale contract สุดท้ายให้สิทธิ์ address ของ crowdsale เป็นคนสร้างเหรียญได้ตรง token.addMinter(crowdsale.address);
9) ลอง compile โค้ดว่าคอมไฟล์ผ่านไหมด้วยคำสั่ง (ถ้าไม่ผ่านก็แก้ตามมันบอก)
truffle compile
10) แก้คอนฟิกไฟล์สำหรับการ migrate (deploy) contract ที่ไฟล์ truffle.js ดังนี้

โดยคอนฟิก development จะชี้ไปที่เครื่องตัวเราเอง เป็นเน็ตเวิร์ค Ethereum ที่เราอยู่บนเครื่องเราคนเดียว
11) รันเน็ตเวิร์ค Ethereum ที่เราจะทดลอง deploy contract ลงไป เปิด terminal ใหม่และรันคำสั่งดังนี้
ganache-cli -u 0
เปิดค้างเอาไว้ จะได้ผลลัพธ์ดังนี้
Image for post
มันจะใช้ user 0 ทำงาน และเราต้องจดหรือก็อปปี้ Mnemonic เอาไว้ ในของผมจะเป็น bike worth melody rescue sudden amateur frown climb swarm response worry xxxx ของคุณเองจะไม่เหมือนของผม มันจะเปลี่ยนไปเรื่อยๆ ถ้ารันใหม่
12) จากนั้นก็เริ่ม migrate contract เข้าไปใน ganache ด้วยคำสั่ง
truffle migrate --reset
จะได้ผลลัพธ์ดังนี้
Image for post
จากผลลัพธ์ด้านบนแปลว่าผมได้สร้าง contract สำเร็จไปที่
Token address: 0x7c9A56E7B62a2b497044094A2eB1AaF26239XXXX
Crowdsale address: 0x2eE021Dc97Fa3B4327d64dd45E53fd69D0D2XXXX
จดสอง Address นี้เอาไว้ ของคุณจะไม่เหมือนกับของผม
12) ต่อไปก็จะลองทดสอบว่าเราโอน Ethereum ไปยัง Crowdsale address แล้วจะได้เหรียญจริงๆ ไหม โดยการลง Browser Extension ชื่อว่า Metamask เป็น กระเป๋าตังค์ Ethereum ที่ใช้งานบนเว็บที่ทำงานบน Ethereum ได้ โหลดที่ https://metamask.io/
13) เปิด Metamask -> Get Started-> Import Wallet
เอา Mnemonic 12 คำที่จดเอาไว้มาวาง ใส่รหัสผ่านให้ตรงกันแล้วกด Import เปลี่ยนเป็น network Localhost:8545 จากมุมบนขวา จะเห็นว่ามีเงินอยู่ในกระเป๋าเกือบ 100 ETH เป็นเงินที่โปรแกรม ganache ใส่มาให้เราทดสอบ มันถูกใช้ไปนิดหน่อยตอนที่เรา migrate contract
Image for post
Image for post
14) กดปุ่ม Add Token เพื่อเพิ่ม TreeCoin แล้ว>ใส่ Token Address ที่จดเอาไว้จากขั้นตอนก่อนหน้า ของผมเป็น 0x7c9A56E7B62a2b497044094A2eB1AaF26239XXXX ถ้าใส่ถูกต้องจะขึ้นชื่อเหรียญ Tree ให้เอง กด Add จะได้ Token Tree เพิ่มขึ้นมา กด Next แล้ว Add Tokens จะเห็นว่ามี 0 Tree เพิ่มขึ้นมา เพราะเรายังไม่ได้ซื้อเหรียญนั่นเอง
Image for post
Image for post
15) ทดลองทำการซื้อหรียญด้วยการโอนเงิน ETH กดปุ่ม SEND ด้านบนขวา แล้วกรอก CrowdSale address ที่จดเอาไว้จากขั้นตอนก่อนหน้านี้มาใส่ ในตัวอย่างเป็น 0x2eE021Dc97Fa3B4327d64dd45E53fd69D0D2e431 กรอกจำนวน ETH ที่จะซื้อ token ในที่นี้จะกรอก 1 ETH ปรับ Advanded Options ให้ใส่ Gas Limit เป็น 210000 กด Save และ Confirm และจะได้เหรียญ BNK มา 20,000 เหรียญตามที่เรากำหนด Rate เอาไว้ ถ้ามีข้อผิดพลาดเช่นซื้อหลังช่วงเวลาที่กำหนด หรืออะไรก็แล้วแต่เงินจะ revert ไม่หายไป แต่มันไม่บอกหรอกนะว่าเพราะอะไรต้องเอา txid มา debug ดูเอาเอง
Image for post
Image for post
Image for post
สรุป



Run
```
npm install -g ganache-cli
npm install -g truffle
mkdir Treecoin-ico && cd Treecoin-ico
truffle init
npm init -Y
npm install --save @openzeppelin/contracts@v2.5 web3
// open Ganache
truffle migrate --reset
```
