
<template>
  <div ref="topbar-container" :class="['topbar-container', {'h-0': !isPinned}]">
    <!-- Add a thin strip at the top that triggers the hover -->
    <div 
      class="hover-zone" 
      @mouseenter="show" 
      style="position: fixed; top: 0; left: 0; width: 100%; height: 10px; z-index: 50;"
    ></div>

    <div class="topbar" :class="{ 'topbar-hidden': !isVisible }" @mouseleave="hide">
      <!-- Rest of your topbar content -->
      <div class="topbar-content">
        <slot name="navigation"></slot>
        <button class="pin-button" @click="togglePin" :title="isPinned ? 'Unpin' : 'Pin'">
          <svg :fill="isPinned ? '#FF0000' : '#000000'" height="24px" width="24px" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
            viewBox="0 0 490.125 490.125" xml:space="preserve">
              <g>
                <path d="M300.625,5.025c-6.7-6.7-17.6-6.7-24.3,0l-72.6,72.6c-6.7,6.7-6.7,17.6,0,24.3l16.3,16.3l-40.3,40.3l-63.5-7
                  c-3-0.3-6-0.5-8.9-0.5c-21.7,0-42.2,8.5-57.5,23.8l-20.8,20.8c-6.7,6.7-6.7,17.6,0,24.3l108.5,108.5l-132.4,132.4
                  c-6.7,6.7-6.7,17.6,0,24.3c3.3,3.3,7.7,5,12.1,5s8.8-1.7,12.1-5l132.5-132.5l108.5,108.5c3.3,3.3,7.7,5,12.1,5s8.8-1.7,12.1-5
                  l20.8-20.8c17.6-17.6,26.1-41.8,23.3-66.4l-7-63.5l40.3-40.3l16.2,16.2c6.7,6.7,17.6,6.7,24.3,0l72.6-72.6c3.2-3.2,5-7.6,5-12.1
                  s-1.8-8.9-5-12.1L300.625,5.025z M400.425,250.025l-16.2-16.3c-6.4-6.4-17.8-6.4-24.3,0l-58.2,58.3c-3.7,3.7-5.5,8.8-4.9,14
                  l7.9,71.6c1.6,14.3-3.3,28.3-13.5,38.4l-8.7,8.7l-217.1-217.1l8.7-8.6c10.1-10.1,24.2-15,38.4-13.5l71.7,7.9
                  c5.2,0.6,10.3-1.2,14-4.9l58.2-58.2c6.7-6.7,6.7-17.6,0-24.3l-16.3-16.3l48.3-48.3l160.3,160.3L400.425,250.025z"/>
              </g>
          </svg>
        </button>
        <Navigation></Navigation>
        <div class="toolbar-button" @mouseleave="hideInfosMenu">
            <div class="relative inline-block">
                <!-- Infos menu positioned above the button -->
                <div v-if="isInfosMenuVisible"  @mouseenter="showInfosMenu" class="absolute m-0 p-0 z-50 top-full right-0 transform bg-white dark:bg-gray-900 rounded-md shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none transition-all duration-300 ease-out mb-2">
                    <div class="p-4 container flex flex-col lg:flex-row items-center gap-2">
                        <!-- SYSTEM STATUS -->
                        <div class="flex gap-3 flex-1 items-center justify-end">
                            <div v-if="isModelOK" title="Model is ok" class="text-green-500 dark:text-green-400 cursor-pointer transition-transform hover:scale-110">
                            <svg class="w-8 h-8" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <path d="M21 12C21 16.9706 16.9706 21 12 21C7.02944 21 3 16.9706 3 12C3 7.02944 7.02944 3 12 3C16.9706 3 21 7.02944 21 12Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                                <path d="M9 12L11 14L15 10" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                            </svg>
                            </div>
                            <div v-else title="Model is not ok" class="text-red-500 dark:text-red-400 cursor-pointer transition-transform hover:scale-110">
                            <svg class="w-8 h-8" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <path d="M21 12C21 16.9706 16.9706 21 12 21C7.02944 21 3 16.9706 3 12C3 7.02944 7.02944 3 12 3C16.9706 3 21 7.02944 21 12Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                                <path d="M15 9L9 15" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                                <path d="M9 9L15 15" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                            </svg>
                            </div>
                            <div v-if="!isGenerating" title="Text is not being generated. Ready to generate" class="text-green-500 dark:text-green-400 cursor-pointer transition-transform hover:scale-110">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 21v-4m0 0V5a2 2 0 012-2h6.5l1 1H21l-3 6 3 6h-8.5l-1-1H5a2 2 0 00-2 2zm9-13.5V9"></path>
                            </svg>
                            </div>
                            <div v-else title="Generation in progress..." class="text-yellow-500 dark:text-yellow-400 cursor-pointer transition-transform hover:scale-110">
                            <svg class="w-6 h-6 animate-spin" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
                            </svg>
                            </div>
                            <div v-if="isConnected" title="Connection status: Connected" class="text-green-500 dark:text-green-400 cursor-pointer transition-transform hover:scale-110">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
                            </svg>
                            </div>
                            <div v-else title="Connection status: Not connected" class="text-red-500 dark:text-red-400 cursor-pointer transition-transform hover:scale-110">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18.364 18.364A9 9 0 005.636 5.636m12.728 12.728A9 9 0 015.636 5.636m12.728 12.728L5.636 5.636"></path>
                            </svg>
                            </div>
                        </div>
                        <div class="flex items-center space-x-4">
                            <ActionButton @click="restartProgram" icon="power" title="restart program" />
                            <ActionButton @click="refreshPage" icon="refresh-ccw" title="refresh page" />
                            <ActionButton href="/docs" icon="file-text" title="Fast API doc" />
                        </div>

                        <!-- SOCIALS -->
                        <SocialIcon href="https://github.com/ParisNeo/lollms-webui" icon="github" />
                        <SocialIcon href="https://www.youtube.com/channel/UCJzrg0cyQV2Z30SQ1v2FdSQ" icon="youtube" />
                        <SocialIcon href="https://x.com/ParisNeo_AI" icon="x" />
                        <SocialIcon href="https://discord.com/channels/1092918764925882418" icon="discord" />
                
                        <div class="relative group" title="Lollms News">
                            <div @click="showNews()" class="text-2xl w-8 h-8 cursor-pointer transition-colors duration-300 text-gray-600 hover:text-primary dark:text-gray-300 dark:hover:text-primary">
                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-full h-full">
                                <path d="M19 20H5a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2v1m2 13a2 2 0 0 1-2-2V7m2 13a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2h-2m-4-3H9M7 16h6M7 8h6v4H7V8z"></path>
                            </svg>
                            </div>
                            <span class="absolute hidden group-hover:block bg-gray-800 text-white text-xs rounded py-1 px-2 top-full left-1/2 transform -translate-x-1/2 mt-2 whitespace-nowrap">
                            Lollms News
                            </span>
                        </div>
                    </div>
                </div>

                <!-- Info Button -->
                <div @mouseenter="showInfosMenu" class="infos-hover-area">
                    <button class="w-6 h-6">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 25 25" class="w-full h-full">
                            <!-- Circle background -->
                            <circle cx="12.5" cy="12.5" r="11.25" fill="#3498db"/>
                            <!-- "i" stem -->
                            <rect x="11.25" y="10" width="2.5" height="8.75" fill="white"/>
                            <!-- "i" dot -->
                            <circle cx="12.5" cy="6.25" r="1.25" fill="white"/>
                        </svg>
                    </button>
                </div>
            </div>      
        </div>
        <div v-if="is_fun_mode" 
                title="Fun mode is on, press to turn off" 
                class="w-8 h-8 cursor-pointer text-green-500 dark:text-green-400 hover:text-green-600 dark:hover:text-green-300 transition-colors duration-300"
                @click="fun_mode_off()"
        >
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-full h-full animate-bounce">
                <circle cx="12" cy="12" r="10"></circle>
                <path d="M8 14s1.5 2 4 2 4-2 4-2"></path>
                <line x1="9" y1="9" x2="9.01" y2="9"></line>
                <line x1="15" y1="9" x2="15.01" y2="9"></line>
                </svg>
            </div>
            <div 
                v-else 
                title="Fun mode is off, press to turn on" 
                class="w-8 h-8 cursor-pointer text-gray-500 dark:text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 transition-colors duration-300"
                @click="fun_mode_on()"
            >
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-full h-full">
                <circle cx="12" cy="12" r="10"></circle>
                <line x1="8" y1="15" x2="16" y2="15"></line>
                <line x1="9" y1="9" x2="9.01" y2="9"></line>
                <line x1="15" y1="9" x2="15.01" y2="9"></line>
                </svg>
            </div>
            <span class="absolute hidden group-hover:block bg-gray-800 text-white text-xs rounded py-1 px-2 top-full left-1/2 transform -translate-x-1/2 mb-2 whitespace-nowrap">
                {{ is_fun_mode ? 'Turn off fun mode' : 'Turn on fun mode' }}
            </span>
        <div class="language-selector relative">
            <button @click="toggleLanguageMenu" class="bg-transparent text-black dark:text-white py-1 px-1 rounded font-bold uppercase transition-colors duration-300 hover:bg-blue-500">
            {{ $store.state.language.slice(0, 2) }}
            </button>
            <div v-if="isLanguageMenuVisible" ref="languageMenu" class="container language-menu absolute left-0 mt-1 bg-white dark:bg-bg-dark-tone rounded shadow-lg z-10 overflow-y-auto scrollbar-thin scrollbar-track-bg-light-tone scrollbar-thumb-bg-light-tone-panel hover:scrollbar-thumb-primary dark:scrollbar-track-bg-dark-tone dark:scrollbar-thumb-bg-dark-tone-panel dark:hover:scrollbar-thumb-primary active:scrollbar-thumb-secondary" style="position: absolute; top: 100%; width: 200px; max-height: 300px; overflow-y: auto;">
            <ul style="list-style-type: none; padding-left: 0; margin-left: 0;">
                <li v-for="language in languages" :key="language" class="relative flex items-center" style="padding-left: 0; margin-left: 0;">
                <button @click="deleteLanguage(language)" class="mr-2 text-red-500 hover:text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-opacity-50 rounded-full">✕</button>
                <div @click="selectLanguage(language)" :class="{'cursor-pointer hover:bg-blue-500 hover:text-white py-2 px-4 block whitespace-no-wrap': true, 'bg-blue-500 text-white': language === $store.state.language, 'flex-grow': true}">
                    {{ language }}
                </div>
                </li>
                <li class="cursor-pointer hover:text-white py-0 px-0 block whitespace-no-wrap">
                <input type="text" v-model="customLanguage" @keyup.enter.prevent="addCustomLanguage" placeholder="Enter language..." class="bg-transparent border border-gray-300 rounded py-0 px-0 mx-0 my-1 w-full">
                </li>
            </ul>
            </div>
        </div>
        <div v-if="isDarkMode" class="sun text-2xl w-6 hover:text-primary duration-150 cursor-pointer" title="Switch to Light theme" @click="themeSwitch()">
            <i data-feather="sun"></i>
        </div>
        <div v-else class="moon text-2xl w-6 hover:text-primary duration-150 cursor-pointer" title="Switch to Dark theme" @click="themeSwitch()">
            <i data-feather="moon"></i>
        </div>             
      </div>
    </div>
  </div>
</template>

<script>
import Navigation from '@/components/Navigation.vue';

export default {
  name: 'TopBar',
  components: {
    Navigation
  },
  data() {
    return {
      isInfosMenuVisible: false,
      isVisible: false,
      isPinned: false,
      selectedLanguage: '',
      isLanguageMenuVisible: false,

    }
  },
  computed:{
    isModelOK(){
      return this.$store.state.isModelOk;
    },
    isDarkMode(){
      return document.documentElement.classList.contains("dark");
    },        
    languages: {
        get(){
            console.log("searching languages", this.$store.state.languages)
            return this.$store.state.languages
        }
    },
    language: {
            get(){
                console.log("searching language", this.$store.state.language)
                return this.$store.state.language
            }
        },
    
    is_fun_mode(){
        try{
            if (this.$store.state.config){
                return this.$store.state.config.fun_mode;
            }
            else{
                return false;
            }
        }
        catch(error){
            console.error("Oopsie! Looks like we hit a snag: ", error);
            return false;
        }
    },
    isGenerating(){
        return this.$store.state.isGenerating;
    },
    isConnected(){
        return this.$store.state.isConnected;
    },     
  },
  methods: {
    themeSwitch() {
        
        if (document.documentElement.classList.contains("dark")) {
            document.documentElement.classList.remove("dark");
            localStorage.setItem("theme", "light")
            this.userTheme == "light"
            this.iconToggle()
          
            return

        }
        import('highlight.js/styles/tokyo-night-dark.css');
        document.documentElement.classList.add("dark");
        localStorage.setItem("theme", "dark")
        this.userTheme == "dark"
        this.iconToggle()
        // Dispatch the themeChanged event
        window.dispatchEvent(new Event('themeChanged'));
    },
    async selectLanguage(language) {
        await this.$store.dispatch('changeLanguage', language);
        this.toggleLanguageMenu(); // Fermer le menu après le changement de langue
        this.language = language
    },
    async deleteLanguage(language) {
        await this.$store.dispatch('deleteLanguage', language);
        this.toggleLanguageMenu(); // Fermer le menu après le changement de langue
        this.language = language
    },
    
    toggleLanguageMenu() {
        console.log("Toggling language ",this.isLanguageMenuVisible)
        this.isLanguageMenuVisible = !this.isLanguageMenuVisible;
    },        

    showInfosMenu() {
        this.isInfosMenuVisible = true;
        nextTick(() => {
            feather.replace()
        })
      },
    hideInfosMenu() {
        this.isInfosMenuVisible = false;
        nextTick(() => {
                feather.replace()
            })
    },    
    show() {
      this.isVisible = true
    },
    hide() {
      if (!this.isPinned) {
        this.isVisible = false
      }
    },
    togglePin() {
      this.isPinned = !this.isPinned
      this.isVisible = this.isPinned
    },
    fun_mode_on(){
        console.log("Turning on fun mode")
        this.$store.state.config.fun_mode=true;
        this.applyConfiguration()
    },
    fun_mode_off(){
        console.log("Turning off fun mode")
        this.$store.state.config.fun_mode=false;
        this.applyConfiguration()
    },
    showNews(){
        this.$store.state.news.show()
        nextTick(() => {
            feather.replace()
        })
    },
    themeCheck() {

        if (this.userTheme == "dark" || (!this.userTheme && this.systemTheme)) {
            document.documentElement.classList.add("dark");
            this.moonIcon.classList.add("display-none");

            nextTick(()=>{
                //import('highlight.js/styles/tokyo-night-dark.css');
                import('highlight.js/styles/stackoverflow-dark.css');

            })

            return
        }

        nextTick(()=>{
            //import('highlight.js/styles/tomorrow-night-blue.css');
            import('highlight.js/styles/stackoverflow-light.css');
        })
        this.sunIcon.classList.add("display-none")

    },
    iconToggle() {
        this.sunIcon.classList.toggle("display-none");
        this.moonIcon.classList.toggle("display-none");
    },        
    refreshPage() {
        const hostnameParts = window.location.href.split('/');

        if(hostnameParts.length > 4){
            window.location.href='/'
        }
        else{
            window.location.reload(true);
        }
    },
    handleOk(inputText) {
        console.log("Input text:", inputText);
    },

  },
}
</script>

<style scoped>
.topbar-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
}

.topbar {
  background-color: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(5px);
  transition: transform 0.3s ease-in-out;
  display: flex;
  justify-content: center;
}

.topbar-hidden {
  transform: translateY(-100%);
}

.topbar-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  width: 100%;
}

.pin-button {
  background-color: transparent;
  border: none;
  cursor: pointer;
  padding: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.pin-button svg {
  width: 24px;
  height: 24px;
  transition: transform 0.3s ease;
}

.pin-button:hover svg {
  transform: scale(1.2);
}

.placeholder {
  height: 10px;
}


.toolbar-button {
@apply bg-transparent border-none cursor-pointer p-2 transition-colors duration-300;
}

.toolbar-button:hover {
@apply text-blue-500; /* Assuming #007bff is close to Tailwind's blue-500 */
}

.topbar-container {
  position: relative;
  width: 100%;
}

.hover-zone {
  opacity: 0;
}
</style>
