<template lang="pug">
q-layout(
  view="hHh lpR ffr"
  @scroll="onScroll"
).whole-bg
  q-header(
    elevated
    :dense="isMobile"
  ).bg-primary.text-white
    q-toolbar(color="primary" style="height: 70px")
      q-btn(
        v-if="isMobile"
        icon="menu"
        aria-label="Drawer Menu"
        flat
        round
        @click="drawerRight = !drawerRight"
      )
      q-btn(
        size="lg"
        unelevated
        no-caps
        to="/"
      )
        q-img(
          src="../assets/images/ossph-logo-text-white.png"
          style="width: 100px"
        )
      q-space
      template(v-if="!isMobile")
        template(v-for="(item, index) in menu")
          template(v-if="item.link")
            q-btn(
              size="lg"
              target="_blank"
              style="min-width: 100px;"
              unelevated
              no-caps
              :icon="item.icon"
              :label="item.name"
              :href="item.link"
            ).q-mr-sm
          template(v-if="item.route")
            q-btn(
              size="lg"
              style="min-width: 100px;"
              unelevated
              no-caps
              :icon="item.icon"
              :label="item.name"
              :to="item.route"
            ).q-mr-sm

  q-drawer(
    v-if="isMobile"
    v-model="drawerRight"
    side="right"
    class="bg-primary"
    show-if-above
    bordered
    :width="300"
    :breakpoint="500"
  )
    q-list
      template(v-for="(item, index) in menu")
        q-item(
          clickable
          v-ripple
          @click="onGoToPanel(item)"
        )
          q-item-section
            q-card(bordered flat)
              q-card-section {{item.name}}
  q-page-container#top
    router-view

  q-footer.text-black.q-pb-md.bg-transparent.text-center
    p.text-body1 Join us on #[a(href="https://discord.com/invite/4ujGbRJyDN" target="_blank").text-primary Discord], and follow up on #[a(href="https://www.facebook.com/ossph.org" target="_blank").text-primary Facebook] and #[a(href="https://twitter.com/OSSPhilippines" target="_blank").text-primary Twitter]
    p.text-body1 #[a(href="https://bit.ly/3xjDvN2" target="_blank").text-primary Press Kit]
</template>

<script>
import { ref, inject, computed } from 'vue';
import { useQuasar } from 'quasar';
import { useRouter } from 'vue-router';

export default {
  setup () {
    const smoothScroll = inject('smoothScroll');
    const $q = useQuasar();
    const isMobile = computed(() => $q.screen.lt.md);
    const router = useRouter();
    const header = ref(false);
    const footer = ref(false);
    const drawerRight = ref(false);
    const menu = ref([
      {
        name: 'The Team',
        route: '/team',
      },
      {
        name: 'Projects',
        route: '/projects',
      },
      {
        name: 'Blog',
        link: 'https://blog.ossph.org',
      },
      {
        name: 'GitHub',
        icon: 'fa-brands fa-github',
        link: 'https://github.com/OSSPhilippines',
      },
    ]);

    if (isMobile.value) header.value = false;

    function onScroll (info) {
      if (info.position >= 150) {
        footer.value = true;
      } else {
        footer.value = false;
      }
      if (isMobile.value) {
        header.value = true;
        return;
      }
      if (info.position >= 150) {
        header.value = true;
      } else {
        header.value = false;
      }
    }

    function onScrollToTop () {
      smoothScroll({
        scrollTo: document.getElementById('top'),
        updateHistory: false,
      });
    }

    function onGoToPanel (card) {
      const panelId = card.panelId;
      drawerRight.value = false;
      if (card.panelId) {
        smoothScroll({
          scrollTo: document.getElementById(panelId),
          updateHistory: false,
        });
      }
      if (card.route) router.push(card.route);
      if (card.link) window.open(card.link, '_blank').focus();
    }

    return {
      drawerRight,
      footer,
      header,
      isMobile,
      menu,
      onScroll,
      onGoToPanel,
      onScrollToTop,
    };
  },
};
</script>

<style scoped>
.whole-bg {
  background: rgb(255,255,255);
  background: radial-gradient(circle, rgba(255,255,255,1) 0%, rgba(227,249,255,1) 100%);
}
</style>
