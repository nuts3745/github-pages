---
layout: "page"
title: "PetShopTutorial"
---
### TruffleのPetShopTutorialをやる
[PetShopTutorial](https://www.trufflesuite.com/tutorials/pet-shop)


### 準備
- NodeとGitを入れる
- npmでtruffleを入れる
- チュートリアル用のディレクトリを作成してそこに移動する
- pet-shopというパッケージをunboxで持ってくる

### ディレクトリ構造
- `contracts/`はSolidityソースファイルを入れておくところ。`Migration.sol`もここにある。
- `migrations/`にはスマートコントラクトをデプロイするときにTruffleが使うやつがおいてある。
- `test/`にはスマートコントラクト用のJavaScriptとSolidityのテストがある。
- `truffle-config.js`はTruffle用のコンフィグファイル。

### スマートコントラクトを書こう
- `contracts/`に`Adoption.sol`を新規作成する。

```solidity
pragma solidity >=0.4.22 <0.6.2;

contract Adoption {
  
}
```

- pragmaでSolidityのコンパイラバージョンを指定する。
  - チュートリアルでは`pragma solidity ^0.5.0;`になってるけど、VSCodeに怒られるので上記のようにいじっている。
  - `pragma solidity ^0.5.0;`は0.5.0かそれ以上のバージョンと言う感じ。
- JavaScriptのようにセミコロン;で終わりを指定する。

### 変数を用意しよう
- Solidityは静的型付言語なので、データタイプをstrとかintとか指定しないといけない。
- また、アドレスというSolidity固有の型がある。
  - EthereumAccountAddressを20byteの値としてストアするもの。
  - Ethereumブロックチェーン上では全てのアカウントとスマートコントラクトがアドレスを持っており、それぞれがそれぞれのアドレスを使用してETHを送受信できる。
- `address[16] public adopters;`を`{}`の中に入れ込む。
  - `adopters`はEthereumAddressesのarrayとして用意した変数。
  - arrayはひとつの型を含み、固定長か可変長で持つことができる。
  - 今回の場合は、型は`address`で、長さは`16`にしてある。
  - `public`となっているが、public変数はブロックチェーンから自動取得することができる。
  - ただ配列の場合はキーを渡して値が返ってくることになるので、あとで配列全体を取得する関数を用意する。

### 最初の関数:ペットを飼う
- adopters変数を定義したあとに次の関数を書いていく。

```solidity
//Adopting pet
    function adopt(uint256 petId) public returns (uint256) {
        require(petId >= 0 && petId <= 15);

        adopters[petId] = msg.sender;

        return petId;
    }
```

- Solidityでは関数での引数も返り値も型を定義しないといけない。
  - この場合も`petId`を定義するときも返すときも`uint256`を指定している。
- `petId`が`adopters`の固定長の配列を越えないように、0から15までを`require()`を使って指定している。
- `petId`が`adopters`配列を越えないのが確認できたら、この関数を呼び出したアドレスを配列内に保存する。
  - `msg.sender`によってこの関数を呼び出したアドレスを示すことができる。