<template>
    <Page>
        <ActionBar>
            <Label text="Календарь"/>
        </ActionBar>
        <FlexboxLayout flexDirection="column" class="calendar-today-box">
            <FlexboxLayout @tap="nowDate" flexDirection="row" class="today-date-info">
                <label :text="dayNow" />
                <label text=" "/>
                <label :text="monthesForNow[month]"/>
                <label :text="year"/>
                <label text=" г."/>
            </FlexboxLayout>
            <FlexboxLayout flexDirection="column" class="calendar-main">
                <FlexboxLayout flexDirection="row" class="month-year-info"> 
                    <FlexboxLayout flexDirection="row">
                        <label :text="monthes[month]"/>
                        <label :text="year"/>
                    </FlexboxLayout>
                    <FlexboxLayout flexDirection="row" class="arrows">
                        <label text="ᐸ" @tap="decrease"/>
                        <label text="ᐳ" @tap="increase"/>
                    </FlexboxLayout>
                </FlexboxLayout>
                <GridLayout class="calendar" rows="auto, auto, auto, auto, auto, auto, auto" columns="*,*,*,*,*,*,*">
                    <label text="Пн" row="0" col="0"/>
                    <label text="Вт" row="0" col="1" />
                    <label text="Ср" row="0" col="2"/>
                    <label text="Чт" row="0" col="3"/>
                    <label text="Пт" row="0" col="4"/>
                    <label text="Сб" row="0" col="5"/>
                    <label text="Вс" row="0" col="6"/>
                </GridLayout>
    
            </FlexboxLayout>
        </FlexboxLayout>
      
        
    </Page>
</template>

<script>
// На главном экране должнен быть чисто календарь
// Нужен селектор выбора месяца, где нибудь сверху как в конвертере
// Текущий день мы должны подсвечивать, также и месяц указывать
// Кнопка возврата к текущему месяцу
  export default{
    data() {
        return {
            month: "",
            year: "",
            dFirstMonth: "1",
            monthesForNow: ["Января ", "Февраля ", "Марта ", "Апреля ", "Мая ", "Июня ", "Июля ", "Августа ", "Сентября ", "Октября ", "Ноября ", "Декабря "],
            monthes: ["Январь ", "Февраль ", "Март ", "Апрель ", "Май ", "Июнь ", "Июль ", "Август ", "Сентябрь ", "Октябрь ", "Ноябрь ", "Декабрь "],
            date: "",
            dayNow: "",
            monthNow: "",
            yearNow: "",

        };
    },
    mounted() {
        // Даты для календаря
        this.month = new Date().getMonth();
        this.year = new Date().getFullYear();
        this.date = new Date();
        // сохраняем даты для последующего возврата к текущей
        this.dayNow = this.date.getDate()
        this.monthNow = this.month;
        this.yearNow = this.year;
    },
    methods: {

        nowDate(){ //возвращаемся к текущей дате

        },

        decrease(){ //уходим на месяц в прошлое
            this.month--;
            if(this.month < 0){
                this.month = 12;
                this.year--;
            }
        },

        increase(){ // на месяц в будущее
            this.month++;
            if(this.month > 11){
                this.month = 0;
                this.year++;
            }
        },

        calendar(){ 

        },
    },
}
  
</script>

<style scoped lang="scss">
    @import '@nativescript/theme/scss/variables/blue';

    // Custom styles
    .fas {
        @include colorize($color: accent);
    }

    .info {
        font-size: 20;
        horizontal-align: center;
        vertical-align: center;
    }
</style>
