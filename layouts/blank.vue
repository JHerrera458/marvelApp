<template>
  <v-app dark>
    <v-card style="margin-bottom: 150px;">
      <v-app-bar absolute color="#fcb69f" dark shrink-on-scroll
        src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/d6862cb9-a6eb-46f4-8df9-411f38ffc20e/da18ugq-f031344b-1c87-4586-a4c7-5d71c308a435.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcL2Q2ODYyY2I5LWE2ZWItNDZmNC04ZGY5LTQxMWYzOGZmYzIwZVwvZGExOHVncS1mMDMxMzQ0Yi0xYzg3LTQ1ODYtYTRjNy01ZDcxYzMwOGE0MzUuanBnIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.bk_lz2tZx3GqPcWInNeT9S-nHdjnmUDfXClIQCTcd8Q"
        scroll-target="#scrolling-techniques-2">
        <template v-slot:img="{ props }">
          <v-img v-bind="props" gradient="to top right, rgba(65, 0, 0, 0.85), rgba(190, 0, 0, 0.85)" width="1920"></v-img>
        </template>

        <v-app-bar-nav-icon></v-app-bar-nav-icon>

        <v-toolbar-title>Marvel APP</v-toolbar-title>

      </v-app-bar>
    </v-card>
    <v-main>
      <Nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'BlankLayout',
  data() {
    return {
    }
  }
}
</script>
