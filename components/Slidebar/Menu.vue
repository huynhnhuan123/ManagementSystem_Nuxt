<script setup lang=ts>
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from '@/components/ui/breadcrumb'
import { 
  Avatar,
  AvatarFallback, 
  AvatarImage } from '@/components/ui/avatar'
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/ui/collapsible'
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu'
import { Separator } from '@/components/ui/separator'
import {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarGroup,
  SidebarGroupLabel,
  SidebarHeader,
  SidebarInset,
  SidebarMenu,
  SidebarMenuAction,
  SidebarMenuButton,
  SidebarMenuItem,
  SidebarMenuSub,
  SidebarMenuSubButton,
  SidebarMenuSubItem,
  SidebarProvider,
  SidebarRail,
  SidebarTrigger,
} from '@/components/ui/sidebar'
import {
  AudioWaveform,
  BadgeCheck,
  Bell,
  ChevronRight,
  ChevronsUpDown,
  Command,
  CreditCard,
  GalleryVerticalEnd,
  PieChart,
  Plus,
  Sparkles,
  SquareTerminal,
  User,
} from 'lucide-vue-next'
import { ref } from 'vue'

import { useAuthStore } from '@/stores/auth';
const authStore = useAuthStore();
const isAuthChecking = ref(true);

onMounted(async () => {
    await authStore.checkAuth();
    isAuthChecking.value = false;
});
const data = {
  // user: {
  //   name: '',
  //   role: '',
  // },
  teams: [
    {
      name: 'Depart 2NH',
      logo: GalleryVerticalEnd,
      plan: 'Corporate Department',
    },
    {
      name: 'Acme Corp.',
      logo: AudioWaveform,
      plan: 'Startup',
    },
    {
      name: 'Evil Corp.',
      logo: Command,
      plan: 'Free',
    },
  ],
  navMain: [
  {
      title: 'Dashboard',
      url: '/',
      icon: PieChart,
      isActive: false,
      items: [
        {
          title: 'Thống kê',
          url: '/',
        },

      ],
    },
    {
      title: 'Đội ngũ',
      url: '/user',
      icon: User,
      isActive: false,
      items: [
        {
          title: 'Thêm tài khoản',
          url: '/user/add',
        },
        {
          title: 'Danh sách tài khoản',
          url: '/user/list',
        },

      ],
    },
    {
      title: 'Phòng ban',
      url: '/depart',
      icon: SquareTerminal,
      isActive: false,
      items: [
        {
          title: 'Thêm phòng ban',
          url: '/depart/add',
        },
        {
          title: 'Danh sách phòng ban',
          url: '/depart/list',
        },

      ],
    },
    {
      title: 'Dự án ',
      url: '/project',
      icon: SquareTerminal,
      isActive: false,
      items: [
        {
          title: 'Thêm dự án',
          url: '/project/add',
        },
        {
          title: 'Danh sách dự án',
          url: '/project/list',
        },

      ],
    },
    {
      title: 'Công việc ',
      url: '/task',
      icon: SquareTerminal,
      isActive: false,
      items: [
        {
          title: 'Thêm công việc',
          url: '/task/add',
        },
        {
          title: 'Danh sách',
          url: '/task/list',
        },

      ],
    },
    
    
  ],

}



const activeTeam = ref(data.teams[0])

function setActiveMenu(title: string) {
  data.navMain.forEach(menu => {
    menu.isActive = false;

    if (menu.title === title) {
      menu.isActive = true; 
    }
  });
} 

function setActiveTeam(team: typeof data.teams[number]) {
  activeTeam.value = team
}

const logout = async () => {
  try {
    console.log('Response:', 1) 
    await fetch('http://localhost:3005/api/user/logout', {
      method: 'POST',
      headers: {'Content-Type': 'application/json'},
      credentials: 'include',
    });
    console.log('Response:', 2) 
    return navigateTo('/auth/login')
  } catch (error) {
    console.error('Logout failed', error);
  }
};




</script>

<template>
  <SidebarProvider>
    <Sidebar collapsible="icon">
      <SidebarHeader>
        <SidebarMenu>
          <SidebarMenuItem>
            <DropdownMenu>
              <DropdownMenuTrigger as-child>
                <SidebarMenuButton size="lg"
                  class="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground">
                  <div
                    class="flex aspect-square size-8 items-center justify-center rounded-lg bg-sidebar-primary text-sidebar-primary-foreground">
                    <component :is="activeTeam.logo" class="size-4" />
                  </div>
                  <div class="grid flex-1 text-left text-sm leading-tight">
                    <span class="truncate font-semibold">{{ activeTeam.name }}</span>
                    <span class="truncate text-xs">{{ activeTeam.plan }}</span>
                  </div>
                  <ChevronsUpDown class="ml-auto" />
                </SidebarMenuButton>
              </DropdownMenuTrigger>
              <DropdownMenuContent class="w-[--radix-dropdown-menu-trigger-width] min-w-56 rounded-lg" align="start"
                side="bottom" :side-offset="4">
                <DropdownMenuLabel class="text-xs text-muted-foreground">
                  Teams
                </DropdownMenuLabel>
                <DropdownMenuItem v-for="(team, index) in data.teams" :key="team.name" class="gap-2 p-2"
                  @click="setActiveTeam(team)">
                  <div class="flex size-6 items-center justify-center rounded-sm border">
                    <component :is="team.logo" class="size-4 shrink-0" />
                  </div>
                  {{ team.name }}
                  <DropdownMenuShortcut>⌘{{ index + 1 }}</DropdownMenuShortcut>
                </DropdownMenuItem>
                <DropdownMenuSeparator />
                <DropdownMenuItem class="gap-2 p-2">
                  <div class="flex size-6 items-center justify-center rounded-md border bg-background">
                    <Plus class="size-4" />
                  </div>
                  <div class="font-medium text-muted-foreground">
                    Add team
                  </div>
                </DropdownMenuItem>
              </DropdownMenuContent>
            </DropdownMenu>
          </SidebarMenuItem>
        </SidebarMenu>
      </SidebarHeader>


      <!--  -->
      <SidebarContent>
  <SidebarGroup>
    <SidebarMenu>
      <Collapsible 
        v-for="item in data.navMain" 
        :key="item.title" 
        as-child 
        :default-open="item.isActive" 
        class="group/collapsible"
      >
        <SidebarMenuItem>
          <CollapsibleTrigger as-child>
            <SidebarMenuButton :tooltip="item.title" @click="setActiveMenu(item.title)">
              <component :is="item.icon" />
              <span>{{ item.title }}</span>
              <ChevronRight 
                class="ml-auto transition-transform duration-200 group-data-[state=open]/collapsible:rotate-90" />
            </SidebarMenuButton>
          </CollapsibleTrigger>
          <CollapsibleContent>
            <SidebarMenuSub>
              <SidebarMenuSubItem v-for="subItem in item.items" :key="subItem.title">
                <SidebarMenuSubButton as-child>
                  <a :href="subItem.url">
                    <span>{{ subItem.title }}</span>
                  </a>
                </SidebarMenuSubButton>
              </SidebarMenuSubItem>
            </SidebarMenuSub>
          </CollapsibleContent>
        </SidebarMenuItem>
      </Collapsible>
    </SidebarMenu>
  </SidebarGroup>
</SidebarContent>

      <SidebarFooter>
        <SidebarMenu>
          <SidebarMenuItem>
            <DropdownMenu>
              <DropdownMenuTrigger as-child>
                <SidebarMenuButton size="lg"
                  class="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground">
                  <Avatar class="h-8 w-8 rounded-lg">
                    <!-- <AvatarImage :src="data.user.avatar" :alt="data.user.name" /> -->
                    <AvatarFallback class="rounded-lg">
                      CN
                    </AvatarFallback>
                  </Avatar>
                  <div v-if="isAuthChecking" class="text-center animate-pulse">
                <div class="h-5 w-20 bg-zinc-600 rounded mb-2"></div>
                <div class="h-8 w-full bg-zinc-600 rounded"></div>
            </div>
            <div class="grid flex-1 text-left text-sm leading-tight">
                      <span class="truncate font-semibold">{{ authStore.user?.username }}</span>
                      <span class="truncate text-xs">{{  authStore.user?.role }}</span>
                    </div>
                  <ChevronsUpDown class="ml-auto size-4" />
                </SidebarMenuButton>
              </DropdownMenuTrigger>
              <DropdownMenuContent class="w-[--radix-dropdown-menu-trigger-width] min-w-56 rounded-lg" side="bottom"
                align="end" :side-offset="4">
                <DropdownMenuLabel class="p-0 font-normal">
                  <div class="flex items-center gap-2 px-1 py-1.5 text-left text-sm">
                    <Avatar class="h-8 w-8 rounded-lg">
                      <!-- <AvatarImage :src="data.user.avatar" :alt="data.user.name" /> -->
                      <AvatarFallback class="rounded-lg">
                        CN
                      </AvatarFallback>
                    </Avatar>
                    <div v-if="isAuthChecking" class="text-center animate-pulse">
                <div class="h-5 w-20 bg-zinc-600 rounded mb-2"></div>
                <div class="h-8 w-full bg-zinc-600 rounded"></div>
            </div>
                    <div class="grid flex-1 text-left text-sm leading-tight">
                      <span class="truncate font-semibold">{{ authStore.user?.username }}</span>
                      <span class="truncate text-xs">{{  authStore.user?.role }}</span>
                    </div>
                  </div>
                </DropdownMenuLabel>
                <DropdownMenuSeparator />
                <DropdownMenuGroup>
                  <DropdownMenuItem>
                    <Sparkles />
                    Upgrade to Pro
                  </DropdownMenuItem>
                </DropdownMenuGroup>
                <DropdownMenuSeparator />
                <DropdownMenuGroup>
                  <DropdownMenuItem>
                    <BadgeCheck />
                    Account
                  </DropdownMenuItem>
                  <DropdownMenuItem>
                    <CreditCard />
                    Billing
                  </DropdownMenuItem>
                  <DropdownMenuItem>
                    <Bell />
                    Notifications
                  </DropdownMenuItem>
                </DropdownMenuGroup>
                <DropdownMenuSeparator />
                <DropdownMenuItem>
                  <a class="nav-link" @click.prevent="logout">Logout</a>
                </DropdownMenuItem>
              </DropdownMenuContent>
            </DropdownMenu>
          </SidebarMenuItem>
        </SidebarMenu>
      </SidebarFooter>
      <SidebarRail />
    </Sidebar>

    <SidebarInset>
      

      <div class="flex flex-1 flex-col gap-4 p-4 pt-0">
        <NuxtPage />
      </div>

    </SidebarInset>

  </SidebarProvider>
</template>


