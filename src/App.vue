<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>버드뷰 </span>
        <span class="font-weight-light">기술검증 테스트</span>
      </v-toolbar-title>
      <v-select v-if="itemDatasOrigin[0]" class="box_select mx-4"
          :items="items"
          v-model="e1"
          label="Select"
          single-line
          auto
          hide-details
          @input='RefreshTable'
        ></v-select>
    </v-app-bar>
    <v-content>
      <v-layout class="mb-4 mt-4" justify-center>
        <v-btn class="mx-4" color="primary" @click="test">api1</v-btn>
        <v-btn class="mx-4" color="primary" @click="test2">ap2</v-btn>
        <v-btn class="mx-4" color="primary" @click="test3">api3</v-btn>
      </v-layout>
      <v-data-table
        :headers="headers"
        :items="itemDatas"
        hide-default-footer
        class="elevation-1"
        :loading="tableLoader"
      >
        <template slot="items" slot-scope="props">
          <td>{{ props.item.name }}</td>
          <td class="text-xs-right">{{ props.item.leftValue }}</td>
          <td class="text-xs-right">{{ props.item.rightValue }}</td>
        </template>
      </v-data-table>
    </v-content>
  </v-app>
</template>
<style scoped>
.box_select {
  max-width: 100px;
}
</style>
<script>
import axios from 'axios';

export default {
  name: 'App',
  data: () => ({
    headers: [
      {
        text: 'matched',
        align: 'left',
        value: 'name'
      },
      { text: 'left', value: 'leftValue' },
      { text: 'right', value: 'rightValue' }
    ],
    itemDatas: [],
    itemDatasOrigin: [],
    e1: null,
    tableLoader:false,
    items: [
      { text: 'A' },
      { text: 'B' },
      { text: 'C' },
      { text: 'D' },
      { text: 'E' },
      { text: 'F' },
      { text: 'G' },
      { text: 'H' },
      { text: 'I' },
      { text: 'J' },
      { text: 'K' },
      { text: 'L' },
      { text: 'M' },
      { text: 'N' },
      { text: 'O' },
      { text: 'P' },
      { text: 'Q' },
      { text: 'R' },
      { text: 'S' },
      { text: 'T' },
      { text: 'U' },
      { text: 'V' },
      { text: 'W' },
      { text: 'X' },
      { text: 'Y' },
      { text: 'Z' }
    ]
  }),
  methods :{
    test(){
      axios.get('https://codetest.hwahae.co.kr/3')
      .then(res => {
        // handle success
        this.tableLoader = true
        this.itemDatas = this.tableData(this.loveLadder(res.data));
        this.itemDatasOrigin = this.tableData(this.loveLadder(res.data));
      })
      .catch(err => {
        // handle error
        console.log(err);
      })
    },
    test2(){
      axios.get('https://codetest.hwahae.co.kr/100')
      .then(res => {
        // handle success
        this.tableLoader = true
        this.itemDatas = this.tableData(this.loveLadder(res.data));
        this.itemDatasOrigin = this.tableData(this.loveLadder(res.data));
      })
      .catch(err => {
        // handle error
        console.log(err)
      })
    },
    test3(){
      axios.get('https://codetest.hwahae.co.kr/10000')
      .then(res => {
        // handle success
        this.tableLoader = true
        this.itemDatas = this.tableData(this.loveLadder(res.data));
        this.itemDatasOrigin = this.tableData(this.loveLadder(res.data));
      })
      .catch(err => {
        // handle error
        console.log(err)
      })
    },
    elemMatcher(elem1, elem2){
        let elem1Array = elem1.split(''); 
        let elem2Array = elem2.split(''); 
        let counter = 0;
        // 첫번째 요소와 두번째 요소 전체 비교
        for(let i=0;i<elem1.length;i++){
            // 첫번째 요소의 원소들과 두번째 요소의 원소 비교
            if(elem2Array.indexOf(elem1Array[i]) != -1){
                counter++
            }
        }
        return counter;
    },
    loveLadder(data){
      const totalArray = [];
      const valueArray = [];
      let result =[];
      for(let i=0;i<data.length;i++){
          let elemArray =[];
          // 특정 요소와 나머지 요소의 비교
          for(let j=i+1;j<data.length;j++){
              elemArray.push(this.elemMatcher(data[i], data[j]));
          }
          // 각 원소의 비교별 최대값
          valueArray[i] = Math.max.apply(null, elemArray);
          // 전체 비교 배열
          totalArray[i] = elemArray;
      }
      // 최대값중의 최대값
      let maxValue = Math.max.apply(null, valueArray);
      for(let k=0;k<totalArray.length;k++){
        if(totalArray[k].indexOf(maxValue) != -1){
            result.push([k+1, k+(totalArray[k].indexOf(maxValue)+2), data[k], data[k+(totalArray[k].indexOf(maxValue)+1)]]);
        }
      }
      this.tableLoader = false
      return result;
    },
    tableData(data){
      let matchingData =[];
      for(let i=0;i<data.length;i++){
          matchingData[i] = {value:false, name:`${data[i][0]}-${data[i][1]}`,leftValue:data[i][2], rightValue:data[i][3] }
        }
      return matchingData;
    },
    RefreshTable(){
      let commonData = [];
      for(let i=0;i<(this.itemDatasOrigin).length;i++){
        if(this.itemDatasOrigin[i].leftValue.split('').indexOf(this.e1) !== -1 && this.itemDatasOrigin[i].rightValue.split('').indexOf(this.e1) !== -1){
          commonData[i] = this.itemDatasOrigin[i];
        }
      }
      this.itemDatas = commonData;
    }
  }
};
</script>
