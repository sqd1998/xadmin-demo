<script setup lang="ts">
import { emitter } from "@/utils/mitt";
import { useNav } from "@/layout/hooks/useNav";
import LaySearch from "../lay-search/index.vue";
import LayNotice from "../lay-notice/index.vue";
import { responsiveStorageNameSpace } from "@/config";
import { ref, nextTick, computed, onMounted } from "vue";
import { storageLocal, isAllEmpty } from "@pureadmin/utils";
import { useTranslationLang } from "../../hooks/useTranslationLang";
import { usePermissionStoreHook } from "@/store/modules/permission";
import LaySidebarItem from "../lay-sidebar/components/SidebarItem.vue";
import LaySidebarFullScreen from "../lay-sidebar/components/SidebarFullScreen.vue";
import GlobalizationIcon from "@/assets/svg/globalization.svg?component";
import AccountSettingsIcon from "~icons/ri/user-settings-line";
import LogoutCircleRLine from "~icons/ri/logout-circle-r-line";
import Setting from "~icons/ri/settings-3-line";
import Check from "~icons/ep/check";

const menuRef = ref();
const showLogo = ref(
  storageLocal().getItem<StorageConfigs>(
    `${responsiveStorageNameSpace()}configure`
  )?.showLogo ?? true
);

const { t, route, locale, translationCh, translationEn } =
  useTranslationLang(menuRef);
const {
  title,
  logout,
  onPanel,
  getLogo,
  username,
  userAvatar,
  backTopMenu,
  avatarsStyle,
  toAccountSettings,
  getDropdownItemStyle,
  getDropdownItemClass
} = useNav();

const defaultActive = computed(() =>
  !isAllEmpty(route.meta?.activePath) ? route.meta.activePath : route.path
);
const mockMenus = [
  {
    path: "/home",
    meta: {
      title: "首页"
    },
    children: [
      {
        path: "/home/ai-portal",
        meta: {
          title: "AI门户"
        }
      }
    ]
  },
  {
    path: "/ability-matrix",
    meta: {
      title: "能力矩阵"
    },
    children: [
      {
        path: "/ability-matrix/huichuang",
        meta: {
          title: "慧创"
        },
        children: [
          {
            path: "/ability-matrix/huichuang/sop-write",
            meta: {
              title: "SOP慧写"
            },
            children: [
              {
                path: "/ability-matrix/huichuang/sop-write/project-management",
                meta: {
                  title: "项目管理"
                },
                children: [
                  {
                    path: "/ability-matrix/huichuang/sop-write/project-management/sop",
                    meta: {
                      title: "SOP项目管理"
                    }
                  },
                  {
                    path: "/ability-matrix/huichuang/sop-write/project-management/docs",
                    meta: {
                      title: "项目文档管理"
                    }
                  }
                ]
              },
              {
                path: "/ability-matrix/huichuang/sop-write/template-maintenance",
                meta: {
                  title: "SOP模板维护"
                },
                children: [
                  {
                    path: "/ability-matrix/huichuang/sop-write/template-maintenance/manage",
                    meta: {
                      title: "模板管理"
                    }
                  }
                ]
              },
              {
                path: "/ability-matrix/huichuang/sop-write/common-tools",
                meta: {
                  title: "通用工具"
                },
                children: [
                  {
                    path: "/ability-matrix/huichuang/sop-write/common-tools/agile-docs",
                    meta: {
                      title: "敏捷文档生成"
                    }
                  }
                ]
              }
            ]
          },
          {
            path: "/ability-matrix/huichuang/aiwrite-external",
            meta: {
              title: "AIWrite分支外链"
            }
          }
        ]
      },
      {
        path: "/ability-matrix/huixi",
        meta: {
          title: "慧析"
        },
        children: [
          {
            path: "/ability-matrix/huixi/knowledge-management",
            meta: {
              title: "知识管理"
            },
            children: [
              {
                path: "/ability-matrix/huixi/knowledge-management/extraction",
                meta: {
                  title: "知识萃取"
                }
              },
              {
                path: "/ability-matrix/huixi/knowledge-management/storage",
                meta: {
                  title: "知识入库"
                }
              }
            ]
          }
        ]
      }
    ]
  },
  {
    path: "/business-platform",
    meta: {
      title: "业务中台"
    },
    children: [
      {
        path: "/business-platform/drug-rd",
        meta: {
          title: "药物研发"
        },
        children: [
          {
            path: "/business-platform/drug-rd/chiral-compounds",
            meta: {
              title: "手性化合物"
            },
            children: [
              {
                path: "/business-platform/drug-rd/chiral-compounds/separation-prediction",
                meta: {
                  title: "分离时间预测"
                }
              },
              {
                path: "/business-platform/drug-rd/chiral-compounds/model-training",
                meta: {
                  title: "模型训练"
                }
              },
              {
                path: "/business-platform/drug-rd/chiral-compounds/prediction-record",
                meta: {
                  title: "分离时间预测与记录"
                }
              },
              {
                path: "/business-platform/drug-rd/chiral-compounds/record",
                meta: {
                  title: "分离时间预测记录"
                }
              }
            ]
          },
          {
            path: "/business-platform/drug-rd/antibody-stability",
            meta: {
              title: "抗体稳定性"
            },
            children: [
              {
                path: "/business-platform/drug-rd/antibody-stability/prediction",
                meta: {
                  title: "稳定性预测"
                }
              }
            ]
          }
        ]
      },
      {
        path: "/business-platform/clinical-rd",
        meta: {
          title: "临床研发"
        },
        children: [
          {
            path: "/business-platform/clinical-rd/patient-matching",
            meta: {
              title: "患者智能匹配"
            },
            children: [
              {
                path: "/business-platform/clinical-rd/patient-matching/project-maintenance",
                meta: {
                  title: "临床项目建立与维护"
                }
              },
              {
                path: "/business-platform/clinical-rd/patient-matching/file-upload",
                meta: {
                  title: "文件上传与处理"
                }
              }
            ]
          },
          {
            path: "/business-platform/clinical-rd/csr-writing",
            meta: {
              title: "CSR撰写"
            },
            children: [
              {
                path: "/business-platform/clinical-rd/csr-writing/project-management",
                meta: {
                  title: "CSR项目管理"
                }
              },
              {
                path: "/business-platform/clinical-rd/csr-writing/template-maintenance",
                meta: {
                  title: "CSR模板维护"
                }
              },
              {
                path: "/business-platform/clinical-rd/csr-writing/report-management",
                meta: {
                  title: "CSR报告管理"
                }
              }
            ]
          }
        ]
      }
    ]
  },
  {
    path: "/data-platform",
    meta: {
      title: "数据中台"
    },
    children: [
      {
        path: "/data-platform/data-insight",
        meta: {
          title: "数据洞察"
        },
        children: [
          {
            path: "/data-platform/data-insight/superset",
            meta: {
              title: "SuperSet"
            }
          },
          {
            path: "/data-platform/data-insight/business-data",
            meta: {
              title: "业务数据"
            }
          }
        ]
      },
      {
        path: "/data-platform/data-governance2",
        meta: {
          title: "数据开发"
        },
        children: [
          {
            path: "/data-platform2/data-governance2/metadata2",
            meta: {
              title: "流程图（开发）"
            }
          },
          {
            path: "/data-platform2/data-governance2/quality2",
            meta: {
              title: "开发系统"
            }
          }
        ]
      },
      {
        path: "/data-platform/data-governance",
        meta: {
          title: "数据配置"
        },
        children: [
          {
            path: "/data-platform/data-governance/metadata",
            meta: {
              title: "数据源配置"
            }
          },
          {
            path: "/data-platform/data-governance/quality",
            meta: {
              title: "应用配置"
            }
          },
          {
            path: "/data-platform/data-governance/quality",
            meta: {
              title: "数据集配置"
            }
          }
        ]
      }
    ]
  },
  {
    path: "/system-management",
    meta: {
      title: "系统管理"
    },
    children: [
      {
        path: "/system-management/user-permission",
        meta: {
          title: "用户权限"
        },
        children: [
          {
            path: "/system-management/user-permission/user",
            meta: {
              title: "用户管理"
            }
          },
          {
            path: "/system-management/user-permission/role",
            meta: {
              title: "角色管理"
            }
          },
          {
            path: "/system-management/user-permission/menu",
            meta: {
              title: "菜单管理"
            }
          }
        ]
      },
      {
        path: "/system-management/system-config",
        meta: {
          title: "系统配置"
        },
        children: [
          {
            path: "/system-management/system-config/params",
            meta: {
              title: "参数设置"
            }
          },
          {
            path: "/system-management/system-config/logs",
            meta: {
              title: "日志管理"
            }
          }
        ]
      }
    ]
  }
];

nextTick(() => {
  menuRef.value?.handleResize();
});

onMounted(() => {
  emitter.on("logoChange", key => {
    showLogo.value = key;
  });
});
</script>

<template>
  <div
    v-loading="usePermissionStoreHook().wholeMenus.length === 0"
    class="horizontal-header"
  >
    <div v-if="showLogo" class="horizontal-header-left" @click="backTopMenu">
      <img :src="getLogo()" alt="logo" />
      <span>{{ title }}</span>
    </div>
    <el-menu
      ref="menuRef"
      mode="horizontal"
      popper-class="pure-scrollbar"
      class="horizontal-header-menu"
      :default-active="defaultActive"
    >
      <LaySidebarItem
        v-for="route in mockMenus"
        :key="route.path"
        :item="route"
        :base-path="route.path"
      />
    </el-menu>
    <div class="horizontal-header-right">
      <!-- 菜单搜索 -->
      <LaySearch id="header-search" />
      <!-- 国际化 -->
      <el-dropdown id="header-translation" trigger="click">
        <div
          class="globalization-icon navbar-bg-hover hover:[&>svg]:animate-scale-bounce"
        >
          <IconifyIconOffline :icon="GlobalizationIcon" />
        </div>
        <template #dropdown>
          <el-dropdown-menu class="translation">
            <el-dropdown-item
              :style="getDropdownItemStyle(locale, 'zh')"
              :class="['dark:text-white!', getDropdownItemClass(locale, 'zh')]"
              @click="translationCh"
            >
              <span v-show="locale === 'zh'" class="check-zh">
                <IconifyIconOffline :icon="Check" />
              </span>
              简体中文
            </el-dropdown-item>
            <el-dropdown-item
              :style="getDropdownItemStyle(locale, 'en')"
              :class="['dark:text-white!', getDropdownItemClass(locale, 'en')]"
              @click="translationEn"
            >
              <span v-show="locale === 'en'" class="check-en">
                <IconifyIconOffline :icon="Check" />
              </span>
              English
            </el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
      <!-- 全屏 -->
      <LaySidebarFullScreen id="full-screen" />
      <!-- 消息通知 -->
      <LayNotice id="header-notice" />
      <!-- 退出登录 -->
      <el-dropdown trigger="click">
        <span class="el-dropdown-link navbar-bg-hover">
          <img :src="userAvatar" :style="avatarsStyle" />
          <p v-if="username" class="dark:text-white">{{ username }}</p>
        </span>
        <template #dropdown>
          <el-dropdown-item @click="toAccountSettings">
            <IconifyIconOffline
              :icon="AccountSettingsIcon"
              style="margin: 5px"
            />
            {{ t("menus.accountSettings") }}
          </el-dropdown-item>
          <el-dropdown-menu class="logout">
            <el-dropdown-item @click="logout">
              <IconifyIconOffline
                :icon="LogoutCircleRLine"
                style="margin: 5px"
              />
              {{ t("buttons.loginOut") }}
            </el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
      <span
        class="set-icon navbar-bg-hover hover:[&>svg]:animate-scale-bounce"
        :title="t('buttons.systemSet')"
        @click="onPanel"
      >
        <IconifyIconOffline :icon="Setting" />
      </span>
    </div>
  </div>
</template>

<style lang="scss" scoped>
:deep(.el-loading-mask) {
  opacity: 0.45;
}

.translation {
  :deep(.el-dropdown-menu__item) {
    padding: 5px 40px;
  }

  .check-zh {
    position: absolute;
    left: 20px;
  }

  .check-en {
    position: absolute;
    left: 20px;
  }
}

.logout {
  width: 120px;

  :deep(.el-dropdown-menu__item) {
    display: inline-flex;
    flex-wrap: wrap;
    min-width: 100%;
  }
}
</style>
