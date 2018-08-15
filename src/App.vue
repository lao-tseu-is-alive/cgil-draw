<style scoped lang="scss">
    .paper {
        background: white;
        border: 1px solid black;
        min-height: 300px;
        min-width: 300px;
    }
    .svg-zoom {
        padding-left: 0.5rem;
    }

</style>

<template>
    <v-app id="inspire">
        <v-navigation-drawer
                :clipped="$vuetify.breakpoint.lgAndUp"
                v-model="drawer"
                :fixed="fixed"
                app
        >
            <v-list dense>
                <template v-for="item in items">
                    <v-layout
                            v-if="item.heading"
                            :key="item.heading"
                            row
                            align-center
                    >
                        <v-flex xs6>
                            <v-subheader v-if="item.heading">
                                {{ item.heading }}
                            </v-subheader>
                        </v-flex>
                        <v-flex xs6 class="text-xs-center">
                            <a href="#!" class="body-2 black--text">EDIT</a>
                        </v-flex>
                    </v-layout>
                    <v-list-group
                            v-else-if="item.children"
                            v-model="item.model"
                            :key="item.text"
                            :prepend-icon="item.model ? item.icon : item['icon-alt']"
                            append-icon=""
                    >
                        <v-list-tile slot="activator">
                            <v-list-tile-content>
                                <v-list-tile-title>
                                    {{ item.text }}
                                </v-list-tile-title>
                            </v-list-tile-content>
                        </v-list-tile>
                        <v-list-tile
                                v-for="(child, i) in item.children"
                                :key="i"
                                @click=""
                        >
                            <v-list-tile-action v-if="child.icon">
                                <v-icon>{{ child.icon }}</v-icon>
                            </v-list-tile-action>
                            <v-list-tile-content>
                                <v-list-tile-title>
                                    {{ child.text }}
                                </v-list-tile-title>
                            </v-list-tile-content>
                        </v-list-tile>
                    </v-list-group>
                    <v-list-tile v-else :key="item.text" @click="">
                        <v-list-tile-action>
                            <v-icon>{{ item.icon }}</v-icon>
                        </v-list-tile-action>
                        <v-list-tile-content>
                            <v-list-tile-title>
                                {{ item.text }}
                            </v-list-tile-title>
                        </v-list-tile-content>
                    </v-list-tile>
                </template>
            </v-list>
        </v-navigation-drawer>
        <v-toolbar
                :clipped-left="$vuetify.breakpoint.lgAndUp"
                color="blue darken-3"
                dark
                app
                :fixed="fixed"
        >
            <v-toolbar-title style="width: 300px" class="ml-0 pl-3">
                <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
                <span class="hidden-sm-and-down">CGil-Draw</span>
            </v-toolbar-title>
            <v-text-field
                    flat
                    solo-inverted
                    hide-details
                    prepend-inner-icon="search"
                    label="Search"
                    class="hidden-sm-and-down"
            ></v-text-field>
            <v-spacer></v-spacer>
            <v-btn icon>
                <v-icon>apps</v-icon>
            </v-btn>
            <v-btn icon>
                <v-icon>notifications</v-icon>
            </v-btn>
            <v-btn icon large>
                <v-avatar size="32px" tile>
                    <img
                            src="https://cdn.vuetifyjs.com/images/logos/logo.svg"
                            alt="Vuetify"
                    >
                </v-avatar>
            </v-btn>
        </v-toolbar>
        <v-content>
            <v-container fluid fill-height>
                <v-layout justify-center align-center class="paper" ref="draw">

                </v-layout>
            </v-container>
        </v-content>
        <v-footer :fixed="fixed" app>
            <span class="svg-wh">WxH: {{`${svgWidth}x${svgHeight}`}}</span>
            <span class="svg-zoom">Zoom : {{`${zoom}`}}</span>
        </v-footer>
        <v-btn
                fab
                bottom
                right
                color="pink"
                dark
                :fixed="fixed"
                @click.stop="dialog = !dialog"
        >
            <v-icon>add</v-icon>
        </v-btn>
        <v-dialog v-model="dialog" width="800px">
            <v-card>
                <v-card-title class="grey lighten-4 py-4 title">
                    Create contact
                </v-card-title>
                <v-container grid-list-sm class="pa-4">
                    <v-layout row wrap>
                        <v-flex xs12 align-center justify-space-between>
                            <v-layout align-center>
                                <v-avatar size="40px" class="mr-3">
                                    <img
                                            src="//ssl.gstatic.com/s2/oz/images/sge/grey_silhouette.png"
                                            alt=""
                                    >
                                </v-avatar>
                                <v-text-field
                                        placeholder="Name"
                                ></v-text-field>
                            </v-layout>
                        </v-flex>
                        <v-flex xs6>
                            <v-text-field
                                    prepend-icon="business"
                                    placeholder="Company"
                            ></v-text-field>
                        </v-flex>
                        <v-flex xs6>
                            <v-text-field
                                    placeholder="Job title"
                            ></v-text-field>
                        </v-flex>
                        <v-flex xs12>
                            <v-text-field
                                    prepend-icon="mail"
                                    placeholder="Email"
                            ></v-text-field>
                        </v-flex>
                        <v-flex xs12>
                            <v-text-field
                                    type="tel"
                                    prepend-icon="phone"
                                    placeholder="(000) 000 - 0000"
                                    mask="phone"
                            ></v-text-field>
                        </v-flex>
                        <v-flex xs12>
                            <v-text-field
                                    prepend-icon="notes"
                                    placeholder="Notes"
                            ></v-text-field>
                        </v-flex>
                    </v-layout>
                </v-container>
                <v-card-actions>
                    <v-btn flat color="primary">More</v-btn>
                    <v-spacer></v-spacer>
                    <v-btn flat color="primary" @click="dialog = false">Cancel</v-btn>
                    <v-btn flat @click="dialog = false">Save</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-app>
</template>

<script>
import Log from 'cgil-log';
import SVG from 'svg.js';
import 'svg.draggable.js';
import 'svg.panzoom.js';

const DEV = process.env.NODE_ENV === 'development';
const MODULE_NAME = 'cgil-draw';
const log = (DEV) ? new Log(MODULE_NAME, 4) : new Log(MODULE_NAME, 1);
let draw = null;

export default {
  name: 'App',

  components: {
  },
  data: () => ({
    params: { radius: 25 },
    zoom: 1,
    svgWidth: null,
    svgHeight: null,
    title: 'SVG.JS',
    circle: null,
    rec: null,
    fixed: true,
    dialog: false,
    drawer: null,
    items: [
      {
        icon: 'contacts',
        text: 'Contacts',
      },
      {
        icon: 'history',
        text: 'Frequently contacted',
      },
      {
        icon: 'content_copy',
        text: 'Duplicates',
      },
      {
        icon: 'keyboard_arrow_up',
        'icon-alt': 'keyboard_arrow_down',
        text: 'Labels',
        model: true,
        children: [
          {
            icon: 'add',
            text: 'Create label',
          },
        ],
      },
      {
        icon: 'keyboard_arrow_up',
        'icon-alt': 'keyboard_arrow_down',
        text: 'More',
        model: false,
        children: [
          { text: 'Import' },
          { text: 'Export' },
          { text: 'Print' },
          { text: 'Undo changes' },
          { text: 'Other contacts' },
        ],
      },
      {
        icon: 'settings',
        text: 'Settings',
      },
      {
        icon: 'chat_bubble',
        text: 'Send feedback',
      },
      {
        icon: 'help',
        text: 'Help',
      },
      {
        icon: 'phonelink',
        text: 'App downloads',
      },
      {
        icon: 'keyboard',
        text: 'Go to the old version',
      },
    ],
  }),
  props: {
    source: String,
  },
  computed: {
    svgCenter2() {
      return { cx: (this.svgWidth / 2), cy: (this.svgHeight / 2) };
    },
    svgCenter() {
      return [(this.svgWidth / 2), (this.svgHeight / 2)];
    },
  },
  mounted() {
    log.t('# IN mounted()');
    draw = SVG(this.$refs.draw).panZoom({ zoomMin: 0.5, zoomMax: 20 });
    draw.on('zoom', (ev) => {
      log.l('zoom event :', ev);
      this.zoom = draw.zoom().toFixed(2);
    })
    log.l('draw SVG.JS :', draw);
    this.svgWidth = draw.viewbox().width;
    this.svgHeight = draw.viewbox().height;
    draw.text(this.title)
      .center(this.svgCenter2.cx, 50)
      .size(25);

    this.circle = draw.circle().radius(this.params.radius)
      .center(...this.svgCenter)
      .fill('#ffff1c')
      .stroke({ color: 'red', width: 2 });

    this.rec = draw.rect(200, 120)
      .center(150, 150)
      .radius(10)
      .fill('rgb(0, 200, 255)')
      .opacity(0.5)
      .draggable((x, y) => ({
        x: x > 10 && x < (this.svgWidth - 210),
        y: y > 10 && y < (this.svgHeight - 130),
      }));
    window.onresize = () => {
      // const w = this.$refs.draw;
      // log.l(`# IN onresize client Width x Height : ${w.clientWidth} x ${w.clientHeight}`);
      this.svgWidth = draw.viewbox().width;
      this.svgHeight = draw.viewbox().height;
      this.circle.center(...this.svgCenter);
    };
  },
};
</script>
