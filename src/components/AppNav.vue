<template>
  <div class="container">
    <div class="header" :class='{"sticky": !desktop}'>
      <div class="row">
        <div class="emblem">
          <router-link to="/">
            <app-image src="https://lh3.google.com/u/2/d/1Ld4exS8QyGD80bKteKeL9mMTtSkRNwbf=w1471-h976-iv1" />
          </router-link>
        </div>

        <p>Відділ освіти, молоді та спорту Вітовської районної державної адміністрації</p>
        <div v-if="desktop" class="emblem">
          <app-image src="https://lh3.google.com/u/2/d/1Uj86F6pUnRwVSExobl4nTqtGBHQ2xhh5=w1285-h976-iv1" />
        </div>

        <div v-if="!desktop" @click="toggleMenu" class="sandwich-icon" :class='{"active": menuIsOpen}'>
          <span class="top"></span>
          <span class="middle"></span>
          <span class="bottom"></span>
        </div>

      </div>
    </div>

    <div v-if="!desktop" class="header spacer">
      <div class="row">
        <div class="emblem">
          <router-link to="/">
            <app-image src="https://lh3.google.com/u/2/d/1Ld4exS8QyGD80bKteKeL9mMTtSkRNwbf=w1471-h976-iv1" />
          </router-link>
        </div>
        <p>Відділ освіти, молоді та спорту Вітовської районної державної адміністрації</p>
        <div class="burger"></div>
      </div>
    </div>

    <transition name="menu">
      <div class="mobile-menu" v-if="menuIsOpen">
        <div class="mobile-links" @click="toggleMenu">
          <div class="main">
            <div class="item" v-for="link in pages" v-if="link.path">
              <router-link class="link" :to="link.path">{{ link.title }}</router-link>
            </div>
          </div>
          <div class="activity">
            <div v-for="link in pages" v-if="link.children">
              <div class="item" v-for="child in link.children">
                <router-link class="link" :to="child.path">{{ child.title }}</router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <div v-if="desktop" class="row">
      <div class="nav">
        <div v-for="page in pages" :key="page.path">

          <router-link v-if="page.path" :to="page.path" v-text="page.title" class="link"/>

          <div v-if="page.children" @mouseenter="mouseenter(page.title)" @mouseleave="mouseleave">
            <span class="link">{{page.title}}</span>
            <transition name="fade">
              <div v-if="hoveredLink === page.title" class="drop">
                <div v-for="child in page.children">
                  <router-link v-if="child.path" :to="child.path" class="link" v-text="child.title"/>
                </div>
              </div>
            </transition>
          </div>

        </div>
      </div>
    </div>

  </div>
</template>

<script>
  import pages from '../pages'

  export default {
    name: "AppNav",
    data() {
      return {
        pages,
        hoveredLink: '',
        desktop: true,
        menuIsOpen: false
      }
    },
    watch: {
      $route() {
        this.mouseleave()
      }
    },
    created() {
      ["orientationchange", "resize"].forEach(e => {
        window.addEventListener(e, this.sizeHandler);
      });

      this.sizeHandler({target: window})
    },
    beforeDestroy() {
      ["orientationchange", "resize"].forEach(e => {
        window.removeEventListener(e, this.sizeHandler);
      });
    },
    methods: {
      mouseenter(link) {
        this.hoveredLink = link;
        this.$emit('fadeContent', true)
      },
      mouseleave() {
        this.hoveredLink = '';
        this.$emit('fadeContent', false)
      },
      sizeHandler({target: {innerWidth}}) {
        this.desktop = innerWidth > 760;
        if (this.desktop) this.menuIsOpen = false;
      },
      toggleMenu() {
        this.menuIsOpen = !this.menuIsOpen;
        this.$emit('toggleMobileMenu', this.menuIsOpen)
      }
    }
  }
</script>

<style scoped lang="scss">
  .container {
    position: relative;
  }

  .header {
    position: relative;
    background: linear-gradient(-2deg, #eecd2b, #36a9e2 35%);
    padding: 8px 0;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.4);
    z-index: 8;

    &.spacer {
      opacity: 0;
      visibility: hidden;
      z-index: 7;
    }

    &.sticky {
      position: fixed;
      top: 0;
      right: 0;
      left: 0;
      padding-left: 16px;
      background: #36a9e2;
    }

    .row {
      max-width: 1200px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    p {
      padding: 0 16px;
      text-transform: uppercase;
      max-width: 900px;

      color: #eee;
      font-size: 26px;
      font-weight: bold;
      line-height: 1.5;
    }

    .emblem {
      width: 100px;
      height: 100px;
      flex-shrink: 0;
      -webkit-filter: drop-shadow(0 1px 4px rgba(0, 0, 0, 0.4)) saturate(0.75);
      filter: drop-shadow(0 1px 4px rgba(0, 0, 0, 0.4)) saturate(0.75);
      overflow: hidden;
      a {
        display: block;
        height: 100%;
        width: 100%;
      }
      img {
        position: relative;
        left: -15px;
        max-width: 155%;
        height: 99%;
      }
    }

    .emblem ~ .emblem {
      img {
        top: 7px;
        left: 15px;
        width: 72%;
        height: 87%;
      }
    }
  }

  .sandwich-icon {
    display: block;
    margin-right: 16px;
    width: 35px;
    height: 22px;
    cursor: pointer;
    flex-shrink: 0;
    z-index: 10;

    & span {
      display: block;
      margin: 0 auto 5px auto;
      width: 82%;
      height: 22%;
      background-color: #eee;
      transition: .3s;
      box-shadow: 0 1px 4px rgba(0, 0, 0, .4);
    }

    &.active {
      .top {
        transform: translateY(10px) translateX(0) rotate(45deg);
      }

      .middle {
        opacity: 0;
      }

      .bottom {
        transform: translateY(-10px) translateX(0) rotate(-45deg);
      }
    }
  }

  .mobile-menu {
    position: absolute;
    top: 100%;
    width: 100%;
    min-height: calc(100vh - 100%);
    background-color: #36a9e2;
    z-index: 9;

    .mobile-links {
      padding: 32px;
      box-sizing: border-box;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
    }

    .main,
    .activity {
      flex-grow: 1;
    }

    .item {
      display: flex;
    }

    .link {
      position: relative;
      width: 100%;
      padding: 12px;
      color: #f5f5f5;

      &:after {
        position: absolute;
        left: 0;
        bottom: 0;
        display: block;
        height: 2px;
        width: 100%;
        content: "";
        background: linear-gradient(90deg, transparent, #ccc 10%, transparent);
      }

      &:hover:after {
        background: linear-gradient(90deg, transparent, #eecd2b 10%, transparent);
      }
    }

    .icon {
      display: flex;
      align-items: center;
      color: #eee;
    }
  }

  .nav {
    position: relative;
    display: flex;
    flex-wrap: wrap;
    z-index: 1;

    & > * {
      flex: 0 1 24%;
    }
  }

  .link {
    padding: 8px 12px;
    height: 100%;
    display: flex;
    align-items: center;
    cursor: pointer;
    position: relative;
    font-family: Montserrat, sans-serif;
    color: #444;

    &:active {
      color: #36a9e2;
    }

    &:after {
      position: absolute;
      bottom: 0;
      display: block;
      height: 2px;
      width: 100%;
      content: "";
      background: linear-gradient(90deg, transparent, #ddd, transparent);
    }

    &:hover:after {
      background: linear-gradient(90deg, transparent, #36a9e2, transparent);
    }
  }

  .drop {
    position: absolute;
    padding: 16px 0;
    background-color: #f5f5f5;

    .link {
      text-align: left;
      justify-content: flex-start;

      &:before {
        display: none;
      }
    }
  }


  @media (max-width: 650px) {
    .header {
      p {
        padding: 0 8px;
        font-size: 12px;
      }

      .emblem {
        width: 64px;
        height: 64px;
      }
    }
  }

</style>
