<template>
    <div class="Pager">
        <nav aria-label="Page navigation">
            <ul class="pagination">
                <li v-bind:class="{disabled:!canPrev()}">
                    <a href="#" aria-label="Назад" v-on:click="prev()">
                        <span aria-hidden="true">&laquo;</span>
                    </a>
                </li>
                <li v-for="n,index in pages"  v-bind:class="{active:currentPage==n}">
                    <a v-if="n>=0" href="#" v-on:click="setCurrentPage(n)">{{ n +1 }}</a>
                    <a v-if="n<0" href="#">...</a>
                </li>

                <li v-bind:class="{disabled:!canNext()}">
                    <a href="#" aria-label="Вперед" v-on:click="next()">
                        <span aria-hidden="true">&raquo;</span>
                    </a>
                </li>
            </ul>
        </nav>
    </div>
</template>

<script>
export default{
    props:['total','toPage'],
    data:function(){
        return{
            currentPage:0,
            currentPositon:0,
            pagesCount:0,
        }
    },
    methods:{
        next:function(){
            if ( this.currentPage < this.pagesCount-1 ){
                this.$parent.$emit("changePage",this.currentPage+1);
                this.currentPage++;
            }
        },
        prev:function(){
            if ( this.currentPage > 0 ){
                this.$parent.$emit("changePage",this.currentPage-1);
                this.currentPage--;
            }

        },
        setCurrentPage:function(p){
            this.$parent.$emit("changePage",p);
            this.currentPage=p;
        },
        canNext:function(){
            return this.currentPage < this.pagesCount-1;
        },
        canPrev:function(){
            return this.currentPage > 0;
        }
    },
    computed:{
        pages:function(){
            let pages=[];
            let limit = 4;
            this.pagesCount = Math.ceil( this.$props.total / this.$props.toPage );

            let diff = this.pagesCount- limit;

            //страниц мало - показываем все
            if ( this.pagesCount<=limit ){
                for ( let i=0; i<this.pagesCount; i++ ){
                    pages.push(i);
                }
            }

            //пограничный случай, когда число страниц незначительно превышает лимит, все равно отображаем все элементы
            if ( this.pagesCount >limit && diff < limit){
                for ( let i=0; i<limit+diff; i++ ){
                    pages.push(i);
                }
            }

            //страниц много больше лимита
            if ( this.pagesCount >limit && diff >= limit){
                pages.push(0);

                let lpart=[];
                let mpart=[];
                let rpart=[];

                let current = this.currentPage+1;
                let total = this.pagesCount-1;

                if ( current < limit ){
                    for(let i=0;i<limit;i++){
                        lpart.push(i);
                    }
                    lpart.push(-1);

                    rpart.push(total);
                }

                if ( current == limit ){
                    for( let i=0; i<limit; i++ ){
                        lpart.push(i);
                    }
                    mpart.push( current+1 );
                    mpart.push( -1 );

                    rpart.push( total );
                }

                if ( current > limit && current <= total-limit ){

                    lpart.push(0);
                    lpart.push(-1);

                    mpart.push(current-2);
                    mpart.push(current-1);
                    mpart.push(current);
                    mpart.push(-1);

                    rpart.push(total);
                }

                if ( current > limit && current > total-limit ){

                    lpart.push(0);
                    lpart.push(-1);


                    for(let i=total-limit;i<total+1;i++){
                        rpart.push(i);
                    }

                }



                pages=lpart.concat(mpart).concat(rpart);
                
            }


            return pages;
        }
    },
    mounted:function(){
        this.pagesCount = Math.ceil( this.$props.total / this.$props.toPage );
        this.$on('changePage',function(pageNumber){
          this.setCurrentPage(pageNumber);
        })
    }
}
</script>

<style>
    .Pager{
        font-size:14pt;
    }
    .pagination>.active>a, .pagination>.active>a:focus, .pagination>.active>a:hover, .pagination>.active>span, .pagination>.active>span:focus, .pagination>.active>span:hover {
        z-index: 3;
        color: #fff;
        cursor: default;
        background-color: #337ab7;
        border-color: #337ab7;}
</style>