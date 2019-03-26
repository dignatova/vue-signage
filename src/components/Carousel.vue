<template>
    <div id="carousel">
      <ul>
        <li v-if="index==activeItem" :class="getItemClass(item.id)" v-for="(item, index) in dsSettings.settings.display.items" >
          <img :src="item.href"/>
        </li>
      </ul>
    </div>
</template>

<script>
  const sleep = (milliseconds) => {
    return new Promise(resolve => setTimeout(resolve, milliseconds))
  }

    export default {
        name: "Carousel",
        data: function () {
          return {
            activeItem: 0,
            hiddenClass: '',
            next: 10,
            nextItem: 0,
            dsSettings: {
              main: {
                company: {
                  id: 123,
                  name: "My Company Name"
                }
              },
              settings: {
                display: {
                  items: [
                    {
                      id: 789,
                      title: '',
                      href: 'https://bipbap.ru/wp-content/uploads/2017/10/0_8eb56_842bba74_XL-640x400.jpg',
                      type: 'picture',
                      mode: {
                        timeout: 10
                      }
                    },
                    {
                      id: 965,
                      title: '',
                      href: 'https://bipbap.ru/wp-content/uploads/2017/10/0_a9e8c_fefaa1d2_XL-640x400.jpg',
                      type: 'picture',
                      mode: {
                        timeout: 15
                      }
                    },
                    {
                      id: 345,
                      title: '',
                      href: 'https://bipbap.ru/wp-content/uploads/2017/10/0_a9e8f_beecb6d9_XL-640x400.jpg',
                      type: 'picture',
                      mode: {
                        timeout: 20
                      }
                    }
                  ]
                }
              }
            }
          }
        },
        created: function() {
          let now = (new Date).getTime();
          this.next = now + this.dsSettings.settings.display.items[0].mode.timeout*1000;
          this.carouselTimer = setInterval(this.changeItem, 1000);
          this.settingsTimer = setInterval(this.updateSettings, 10000);

        },
        methods: {

          changeItem: function () {
            let now = (new Date).getTime();

            if (this.next  &&  this.next < now) {

              this.nextItem = (this.activeItem == this.dsSettings.settings.display.items.length - 1) ? 0 : this.activeItem+1;
              this.next = now + this.dsSettings.settings.display.items[this.nextItem].mode.timeout * 1000;
              this.hiddenClass = 'hidden';

              sleep(2000).then(() => {

                this.activeItem = this.nextItem;
                this.hiddenClass = 'active';

              });
            }


          },

          updateSettings: function () {
            //TODO: Get dsSettings object with Axios.
          },

          getItemClass(id) {
            return 'carousel-item item-'+id+' '+this.hiddenClass;
          }
        }
    }
</script>

<style scoped>
  #carousel {
    background-color: #111;
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
  }

  .carousel-item {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: #111;
    transition: all 1s linear;
  }

  .carousel-item img {
    width: 100%;
    height: 100%;
  }

  .hidden {
    opacity: 1;
    //transition: all 2s linear;
    transition: 2s;
    animation: hide 3s 1;
    animation-fill-mode: forwards;
  }

  .active {
    opacity: 0;
    transition: 2s;
    animation: show 3s 1;
    animation-fill-mode: forwards;
  }

  @keyframes show{
    0%{
      opacity:0;
    }

    100% {
      opacity:1;
    }
  }

  @keyframes hide{
    0%{
      opacity:1;
    }

    100% {
      opacity:0;
    }

  }

</style>
