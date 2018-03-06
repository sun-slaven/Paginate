<template lang="html">
    <nav class="zen-pagination snap-paginate">
		<ul>
      <!-- 前一页 -->
  		<li
        data-role="prev"
        :class="{'disabled': currentPage === 1}"
        @click="choosePrevPage">
  			<a><span class="zen-icon-caret-left"></span></a>
  		</li>
      <!-- 第一页 -->
      <li v-if="showFirstPage && showEllipsis && !ifcontinousPageArrayContains(1)" @click="choosePage(1)">
          <a>1</a>
      </li>
      <!-- ... 若第二页不存在于continousPageArray则显示-->
      <li
        v-if="showEllipsis && !ifcontinousPageArrayContains(2)"
        @click="choosePrevEllipsis"
        :class="{'disable-click': !canClickEllipsis}">
        <a><span class="zen-icon-ellipsis"></span></a>
      </li>
      <!-- 中间连续页数 -->
      <li
        v-for="page in continousPageArray"
        :class="{'selected': page === currentPage}"
        @click="choosePage(page)"
        data-role="page">
        <a>{{page}}</a>
      </li>
      <!-- ... 若倒数第二页不存在于continousPageArray则显示-->
      <li
        v-if="showEllipsis && !ifcontinousPageArrayContains(maxPage - 1)"
        @click="chooseNextEllipsis"
        :class="{'disable-click': !canClickEllipsis}">
        <a><span class="zen-icon-ellipsis"></span></a>
      </li>
      <!-- 最后一页 -->
      <li v-if="showLastPage && showEllipsis && !ifcontinousPageArrayContains(maxPage)" @click="choosePage(maxPage)">
          <a>{{maxPage}}</a>
      </li>
      <!-- 后一页 -->
			<li
        data-role="next"
        :class="{'disabled': currentPage === maxPage}"
        @click="chooseNextPage">
				<a><span class="zen-icon-caret-right"></span></a>
			</li>
		</ul>
        <!-- 跳页 -->
		<div class="zen-input-with-addon" data-role="goto" v-if="showPageInput">
			<input type="text" v-model="pageInput" @keyup.enter="goPage"></input>
			<button type="button" data-role="addon" @click="goPage">Go</button>
		</div>
 	</nav>
</template>

<script>
import generateContinousNumberArray from 'library-@/utils/generate-continous-number-array.js';

export default {
  name: 'SnapPaginate',
  props: {
    'maxPage': {
      required: false,
      default: 100
    },
    // 最大连续数量
    'continousNumber': {
      required: false,
      default: 10
    },
    'currentPage': {
      required: true,
      default: 1
    },
    'showFirstPage': {
      type: Boolean,
      required: false,
      default: true
    },
    'showLastPage': {
      type: Boolean,
      required: false,
      default: true
    },
    'showEllipsis': {
      type: Boolean,
      required: false,
      default: true
    },
    'canClickEllipsis': {
      type: Boolean,
      required: false,
      default: true
    },
    'showPageInput': {
      type: Boolean,
      required: false,
      default: true
    },
    'choosePageCallback': {
      type: Function,
      required: true
    }
  },
  data () {
    return {
      pageInput: ''
    };
  },
  computed: {
    // eg: 10 => {left: 4, right: 5} ,9 => {left: 4,right: 4}
    // 连续页数左半部分的页码个数,奇数时保证当前页在中间,自己占据一个li
    leftHalfContinusNumber () {
      return Math.floor((this.continousNumber - 1) / 2);
    },
    // 连续页数右半部分的页码个数
    rightHalfContinusNumber () {
      return this.continousNumber - this.leftHalfContinusNumber - 1;
    },
    // 中间连续页数码数组
    // 各种情况分析:
    // [1...5...maxPage] 只有几页
    // [1...5...10] ... 120 //1 [6-4...6...10] ... 120
    // 1 ... [7-4...7...7+5] ... 120   // 1 ... [11-4...11,11+5] ... 120  // 1 ... [113-4...113,113+5] ... 120
    // 1 ... [114-4...114,114+5] 120  // 1 ... [115-4...115,115+5] // 1 ... [120-10...117,120]
    continousPageArray () {
      if (this.maxPage <= this.continousNumber) {
        return generateContinousNumberArray(1, this.maxPage);
      } else if (this.currentPage <= this.leftHalfContinusNumber + 2) { // case1: 只显示右边的...和尾页
        return generateContinousNumberArray(1, this.continousNumber);
      } else if (this.currentPage < this.maxPage - this.rightHalfContinusNumber) { // case2: 左边和右边的...、首尾页都显示
        return generateContinousNumberArray(this.currentPage - this.leftHalfContinusNumber, this.currentPage + this.rightHalfContinusNumber);
      } else { // case3: 只显示左边的...和首页
        return generateContinousNumberArray(this.maxPage - this.continousNumber + 1, this.maxPage);
      }
    },
    canShowPreEllipsis () {
      if (this.showEllipsis) {
        if (this.maxPage && this.maxPage > this.visiablemaxPage) {
          return true;
        }
      }
      return false;
    },
    canShowNextEllipsis () {
      if (this.showEllipsis) {
        if (this.maxPage && this.maxPage > this.visiablemaxPage) {
          return true;
        }
      }
      return false;
    },
    canshowPageInput () {
      if (this.showPageInput) {
        if (this.canShowPreEllipsis || this.canShowNextEllipsis) {
          return true;
        }
      }
      return false;
    }
  },
  methods: {
    choosePage (pageNumber) {
      if (typeof pageNumber !== 'number') return;
      if (pageNumber > this.maxPage || pageNumber < 1 || pageNumber === this.currentPage) {
        return;
      } else {
        this.choosePageCallback(pageNumber);
      }
    },
    choosePrevPage () {
      this.choosePage(this.currentPage - 1);
    },
    chooseNextPage () {
      this.choosePage(this.currentPage + 1);
    },
    choosePrevEllipsis () {
      if (this.canClickEllipsis) {
        this.choosePage(1 + this.leftHalfContinusNumber);
      }
    },
    chooseNextEllipsis () {
      if (this.canClickEllipsis) {
        this.choosePage(this.maxPage - this.rightHalfContinusNumber);
      }
    },
    goPage () {
      var pageStr = this.pageInput.trim();
      var pageNumber = Number(pageStr);
      this.pageInput = '';
      this.choosePage(pageNumber);
    },
    ifcontinousPageArrayContains (page) {
      // page < 1 || this.maxPage <= page 是 只有几页时 的判断条件
      return page < 1 || this.maxPage <= page || _.includes(this.continousPageArray, page);
    }
  }
};
</script>
