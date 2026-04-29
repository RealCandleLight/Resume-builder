<template>
  <div class="app-container">
    <h1>📄 简历制作</h1>
    
    <div class="main-content">
      <!-- 左侧表单 -->
      <div class="left-panel">
        <h2>填写信息</h2>
        
        <h3>基本信息</h3>
        <el-input v-model="resumeData.name" placeholder="姓名" style="margin-bottom: 10px" />
        <el-input v-model="resumeData.jobTitle" placeholder="求职岗位" style="margin-bottom: 10px" />
        
        <el-row :gutter="10" style="margin-bottom: 10px">
          <el-col :span="12">
            <el-input v-model="resumeData.birth" placeholder="出生年月" />
          </el-col>
          <el-col :span="12">
            <el-input v-model="resumeData.gender" placeholder="性别" />
          </el-col>
        </el-row>

        <el-row :gutter="10" style="margin-bottom: 10px">
          <el-col :span="12">
            <el-input v-model="resumeData.phone" placeholder="手机号" />
          </el-col>
          <el-col :span="12">
            <el-input v-model="resumeData.email" placeholder="邮箱" />
          </el-col>
        </el-row>

        <el-input v-model="resumeData.city" placeholder="所在城市" style="margin-bottom: 10px" />
        <el-input v-model="resumeData.website" placeholder="个人网站/GitHub（可选）" style="margin-bottom: 10px" />
        <el-input v-model="resumeData.wechat" placeholder="微信号（可选）" style="margin-bottom: 10px" />

        <h3>头像上传</h3>
        <div class="avatar-upload">
          <input type="file" accept="image/*" @change="handleAvatarUpload" ref="fileInput" style="display: none" />
          <el-button @click="$refs.fileInput.click()">选择图片</el-button>
          <img v-if="resumeData.avatar" :src="resumeData.avatar" class="avatar-preview" />
        </div>

        <h3>自定义信息</h3>
        <div v-for="(field, index) in resumeData.customFields" :key="index" class="custom-field">
          <el-row :gutter="10" style="margin-bottom: 10px">
            <el-col :span="10">
              <el-input v-model="field.label" placeholder="字段名 如：政治面貌" />
            </el-col>
            <el-col :span="10">
              <el-input v-model="field.value" placeholder="内容 如：中共党员" />
            </el-col>
            <el-col :span="4">
              <el-button type="danger" size="small" @click="removeCustomField(index)" circle>✕</el-button>
            </el-col>
          </el-row>
        </div>
        <el-button type="primary" @click="addCustomField" size="small">+ 添加自定义信息</el-button>

        <h3>补充介绍</h3>
        <el-input 
          v-model="resumeData.summary" 
          type="textarea" 
          :rows="4"
          placeholder="简短的个人介绍，突出你的优势和职业目标（可选）" 
          style="margin-bottom: 10px"
        />

        <h3>教育经历</h3>
        <el-input v-model="resumeData.education.school" placeholder="学校名称" style="margin-bottom: 10px" />
        <el-input v-model="resumeData.education.major" placeholder="专业" style="margin-bottom: 10px" />
        <el-input v-model="resumeData.education.degree" placeholder="学历" style="margin-bottom: 10px" />
        <el-input v-model="resumeData.education.eduTime" placeholder="就读时间 如：2020-2024" style="margin-bottom: 10px" />
        <el-input v-model="resumeData.education.gpa" placeholder="GPA/成绩（可选）" style="margin-bottom: 10px" />
        <el-input v-model="resumeData.education.courses" placeholder="主修课程（可选，用逗号分隔）" style="margin-bottom: 10px" />

        <h3>工作经历</h3>
        <div v-for="(work, index) in resumeData.works" :key="index" class="dynamic-form">
          <el-input v-model="work.company" placeholder="公司名称" style="margin-bottom: 10px" />
          <el-input v-model="work.position" placeholder="职位" style="margin-bottom: 10px" />
          <el-input v-model="work.workTime" placeholder="工作时间 如：2024.07-至今" style="margin-bottom: 10px" />
          <el-input v-model="work.description" type="textarea" :rows="3" placeholder="工作描述" style="margin-bottom: 10px" />
          <el-button type="danger" size="small" @click="removeWork(index)" v-if="resumeData.works.length > 1">删除此工作</el-button>
          <hr style="margin: 10px 0" />
        </div>
        <el-button type="primary" @click="addWork">+ 添加工作经历</el-button>

        <h3>项目经验</h3>
        <div v-for="(project, index) in resumeData.projects" :key="index" class="dynamic-form">
          <el-input v-model="project.name" placeholder="项目名称" style="margin-bottom: 10px" />
          <el-input v-model="project.role" placeholder="负责角色" style="margin-bottom: 10px" />
          <el-input v-model="project.tech" placeholder="技术栈" style="margin-bottom: 10px" />
          <el-input v-model="project.description" type="textarea" :rows="2" placeholder="项目描述" style="margin-bottom: 10px" />
          <el-button type="danger" size="small" @click="removeProject(index)" v-if="resumeData.projects.length > 1">删除此项目</el-button>
          <hr style="margin: 10px 0" />
        </div>
        <el-button type="primary" @click="addProject">+ 添加项目经验</el-button>

        <h3>竞赛经历</h3>
        <div v-for="(competition, index) in resumeData.competitions" :key="index" class="dynamic-form">
          <el-input v-model="competition.name" placeholder="竞赛名称" style="margin-bottom: 10px" />
          <el-input v-model="competition.award" placeholder="获奖情况 如：一等奖" style="margin-bottom: 10px" />
          <el-input v-model="competition.time" placeholder="竞赛时间" style="margin-bottom: 10px" />
          <el-input v-model="competition.description" type="textarea" :rows="2" placeholder="竞赛描述（可选）" style="margin-bottom: 10px" />
          <el-button type="danger" size="small" @click="removeCompetition(index)" v-if="resumeData.competitions.length > 1">删除此竞赛</el-button>
          <hr style="margin: 10px 0" />
        </div>
        <el-button type="primary" @click="addCompetition">+ 添加竞赛经历</el-button>

        <h3>资格认证</h3>
        <div v-for="(cert, index) in resumeData.certifications" :key="index" class="dynamic-form">
          <el-input v-model="cert.name" placeholder="证书名称" style="margin-bottom: 10px" />
          <el-input v-model="cert.organization" placeholder="颁发机构" style="margin-bottom: 10px" />
          <el-input v-model="cert.time" placeholder="获得时间" style="margin-bottom: 10px" />
          <el-button type="danger" size="small" @click="removeCertification(index)" v-if="resumeData.certifications.length > 1">删除此证书</el-button>
          <hr style="margin: 10px 0" />
        </div>
        <el-button type="primary" @click="addCertification">+ 添加资格认证</el-button>

        <h3>技能特长</h3>
        <el-input v-model="skillInput" placeholder="输入技能后按回车添加" @keyup.enter="addSkill" style="margin-bottom: 10px" />
        <div class="tag-list">
          <el-tag v-for="(skill, index) in resumeData.skills" :key="index" closable @close="removeSkill(index)" style="margin: 0 5px 5px 0">
            {{ skill }}
          </el-tag>
        </div>

        <h3>模块排序</h3>
        <p class="sort-tip">拖拽调整简历模块顺序，开关控制显示/隐藏</p>
        <draggable v-model="resumeData.moduleOrder" item-key="id" class="sort-list">
          <template #item="{ element }">
            <div class="sort-item">
              <span class="drag-handle">☰</span>
              <span>{{ moduleNameMap[element] }}</span>
              <el-switch v-model="resumeData.moduleVisible[element]" size="small" />
            </div>
          </template>
        </draggable>
      </div>

      <!-- 右侧预览 -->
      <div class="right-panel">
        <h2>简历预览</h2>
        <div style="display: flex; gap: 10px; margin-bottom: 10px;">
          <el-button type="warning" @click="saveData">💾 保存数据</el-button>
          <el-button type="success" @click="exportPDF">📥 导出PDF</el-button>
        </div>
        <div id="resume-preview" class="resume-paper">
          
          <!-- 头部 -->
          <div class="resume-header">
            <img v-if="resumeData.avatar" :src="resumeData.avatar" class="resume-avatar" />
            <h1>{{ resumeData.name || '你的姓名' }}</h1>
            <p class="job-title">{{ resumeData.jobTitle || '求职岗位' }}</p>
            <div class="contact-info">
              <span v-if="resumeData.phone">📱 {{ resumeData.phone }}</span>
              <span v-if="resumeData.email">📧 {{ resumeData.email }}</span>
            </div>
            <div class="contact-info secondary" style="margin-top: 8px">
              <span v-if="resumeData.birth">{{ resumeData.birth }}</span>
              <span v-if="resumeData.gender">{{ resumeData.gender }}</span>
              <span v-if="resumeData.city">📍 {{ resumeData.city }}</span>
            </div>
            <div class="contact-info secondary" v-if="resumeData.website || resumeData.wechat">
              <span v-if="resumeData.website">🔗 {{ resumeData.website }}</span>
              <span v-if="resumeData.wechat">💬 {{ resumeData.wechat }}</span>
            </div>
            <!-- 自定义信息 -->
            <div class="contact-info secondary" v-if="resumeData.customFields.some(f => f.label && f.value)">
              <span v-for="field in resumeData.customFields" :key="field.label">
                <template v-if="field.label && field.value">{{ field.label }}：{{ field.value }}</template>
              </span>
            </div>
          </div>

          <!-- 补充介绍 -->
          <div class="section summary-section" v-if="resumeData.summary && resumeData.moduleVisible['summary']">
            <h3>📋 个人介绍</h3>
            <p class="summary-text">{{ resumeData.summary }}</p>
          </div>

          <!-- 按顺序渲染模块 -->
          <template v-for="moduleName in orderedVisibleModules" :key="moduleName">
            
            <div v-if="moduleName === 'education' && resumeData.education.school" class="section">
              <h3>📚 教育经历</h3>
              <div class="item">
                <div class="item-header">
                  <strong>{{ resumeData.education.school }}</strong>
                  <span>{{ resumeData.education.eduTime }}</span>
                </div>
                <p>{{ resumeData.education.major }} · {{ resumeData.education.degree }}</p>
                <p v-if="resumeData.education.gpa" class="description">GPA：{{ resumeData.education.gpa }}</p>
                <p v-if="resumeData.education.courses" class="description">主修课程：{{ resumeData.education.courses }}</p>
              </div>
            </div>

            <div v-if="moduleName === 'work' && resumeData.works.some(w => w.company)" class="section">
              <h3>💼 工作经历</h3>
              <div v-for="work in resumeData.works" :key="work.company" class="item">
                <div v-if="work.company">
                  <div class="item-header">
                    <strong>{{ work.company }}</strong>
                    <span>{{ work.workTime }}</span>
                  </div>
                  <p>{{ work.position }}</p>
                  <p class="description">{{ work.description }}</p>
                </div>
              </div>
            </div>

            <div v-if="moduleName === 'projects' && resumeData.projects.some(p => p.name)" class="section">
              <h3>🚀 项目经验</h3>
              <div v-for="project in resumeData.projects" :key="project.name" class="item">
                <div v-if="project.name">
                  <div class="item-header">
                    <strong>{{ project.name }}</strong>
                    <span>{{ project.role }}</span>
                  </div>
                  <p>技术栈：{{ project.tech }}</p>
                  <p class="description">{{ project.description }}</p>
                </div>
              </div>
            </div>

            <div v-if="moduleName === 'competitions' && resumeData.competitions.some(c => c.name)" class="section">
              <h3>🏆 竞赛经历</h3>
              <div v-for="competition in resumeData.competitions" :key="competition.name" class="item">
                <div v-if="competition.name">
                  <div class="item-header">
                    <strong>{{ competition.name }}</strong>
                    <span class="award">{{ competition.award }}</span>
                  </div>
                  <p>{{ competition.time }}</p>
                  <p class="description" v-if="competition.description">{{ competition.description }}</p>
                </div>
              </div>
            </div>

            <div v-if="moduleName === 'certifications' && resumeData.certifications.some(c => c.name)" class="section">
              <h3>📜 资格认证</h3>
              <div v-for="cert in resumeData.certifications" :key="cert.name" class="item">
                <div v-if="cert.name">
                  <div class="item-header">
                    <strong>{{ cert.name }}</strong>
                    <span>{{ cert.time }}</span>
                  </div>
                  <p v-if="cert.organization">颁发机构：{{ cert.organization }}</p>
                </div>
              </div>
            </div>

            <div v-if="moduleName === 'skills' && resumeData.skills.length > 0" class="section">
              <h3>🛠 技能特长</h3>
              <div class="skill-tags">
                <span v-for="skill in resumeData.skills" :key="skill" class="skill-tag">{{ skill }}</span>
              </div>
            </div>

          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'
import { ElMessage } from 'element-plus'
import html2canvas from 'html2canvas'
import jsPDF from 'jspdf'
import draggable from 'vuedraggable'

const resumeData = reactive({
  name: '',
  jobTitle: '',
  birth: '',
  gender: '',
  phone: '',
  email: '',
  city: '',
  website: '',
  wechat: '',
  avatar: '',
  summary: '',
  customFields: [],
  education: { school: '', major: '', degree: '', eduTime: '', gpa: '', courses: '' },
  works: [{ company: '', position: '', workTime: '', description: '' }],
  projects: [{ name: '', role: '', tech: '', description: '' }],
  competitions: [{ name: '', award: '', time: '', description: '' }],
  certifications: [{ name: '', organization: '', time: '' }],
  skills: [],
  moduleOrder: ['summary', 'education', 'work', 'projects', 'competitions', 'certifications', 'skills'],
  moduleVisible: {
    summary: true,
    education: true,
    work: true,
    projects: true,
    competitions: true,
    certifications: true,
    skills: true
  }
})

const moduleNameMap = {
  summary: '个人介绍',
  education: '教育经历',
  work: '工作经历',
  projects: '项目经验',
  competitions: '竞赛经历',
  certifications: '资格认证',
  skills: '技能特长'
}

const orderedVisibleModules = computed(() => {
  return resumeData.moduleOrder.filter(m => resumeData.moduleVisible[m])
})

const skillInput = ref('')

const handleAvatarUpload = (event) => {
  const file = event.target.files[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (e) => { resumeData.avatar = e.target.result }
    reader.readAsDataURL(file)
  }
}

const addCustomField = () => resumeData.customFields.push({ label: '', value: '' })
const removeCustomField = (index) => resumeData.customFields.splice(index, 1)

const addSkill = () => {
  if (skillInput.value.trim()) {
    resumeData.skills.push(skillInput.value.trim())
    skillInput.value = ''
  }
}
const removeSkill = (index) => resumeData.skills.splice(index, 1)

const addWork = () => resumeData.works.push({ company: '', position: '', workTime: '', description: '' })
const removeWork = (index) => resumeData.works.splice(index, 1)
const addProject = () => resumeData.projects.push({ name: '', role: '', tech: '', description: '' })
const removeProject = (index) => resumeData.projects.splice(index, 1)
const addCompetition = () => resumeData.competitions.push({ name: '', award: '', time: '', description: '' })
const removeCompetition = (index) => resumeData.competitions.splice(index, 1)
const addCertification = () => resumeData.certifications.push({ name: '', organization: '', time: '' })
const removeCertification = (index) => resumeData.certifications.splice(index, 1)

// ========== 本地存储 ==========

// 保存数据
const saveData = () => {
  const dataStr = JSON.stringify(resumeData)
  localStorage.setItem('resume-builder-data', dataStr)
  ElMessage.success('数据已保存到本地！')
}

// 页面加载时自动读取
const savedData = localStorage.getItem('resume-builder-data')
if (savedData) {
  try {
    const parsed = JSON.parse(savedData)
    Object.assign(resumeData, parsed)
  } catch (e) {
    console.log('读取数据失败')
  }
}

const exportPDF = async () => {
  const element = document.getElementById('resume-preview')
  const canvas = await html2canvas(element, { scale: 2, useCORS: true })
  const imgData = canvas.toDataURL('image/png')
  const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' })
  const imgWidth = 210
  const imgHeight = (canvas.height * imgWidth) / canvas.width
  pdf.addImage(imgData, 'PNG', 0, 0, imgWidth, imgHeight)
  pdf.save('我的简历.pdf')
}
</script>

<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
.app-container { height: 100vh; background: #f0f2f5; padding-top: 20px; }
.app-container > h1 { text-align: center; color: #333; margin-bottom: 20px; }
.main-content { display: flex; height: calc(100vh - 80px); padding: 0 20px; gap: 20px; }
.left-panel { width: 440px; background: white; padding: 20px; border-radius: 8px; overflow-y: auto; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }
.left-panel h2 { margin-bottom: 20px; color: #333; }
.left-panel h3 { margin: 15px 0 10px; color: #666; border-bottom: 1px solid #eee; padding-bottom: 5px; }
.tag-list { margin-bottom: 10px; }
.dynamic-form { border: 1px solid #eee; padding: 10px; border-radius: 8px; margin-bottom: 10px; background: #fafafa; }
.custom-field { margin-bottom: 5px; }
.right-panel { flex: 1; background: white; padding: 20px; border-radius: 8px; display: flex; flex-direction: column; align-items: center; box-shadow: 0 2px 8px rgba(0,0,0,0.1); overflow-y: auto; }
.resume-paper { width: 210mm; min-height: 297mm; padding: 25mm 20mm; background: white; border: 1px solid #ddd; box-shadow: 0 4px 12px rgba(0,0,0,0.15); margin-top: 20px; }
.resume-header { text-align: center; margin-bottom: 30px; padding-bottom: 20px; border-bottom: 2px solid #333; position: relative; }
.resume-header h1 { font-size: 28px; margin-bottom: 5px; }
.job-title { font-size: 16px; color: #666; margin-bottom: 10px; }
.contact-info { font-size: 13px; color: #666; display: flex; justify-content: center; gap: 15px; flex-wrap: wrap; }
.contact-info.secondary { font-size: 12px; color: #888; gap: 12px; }
.resume-avatar { width: 80px; height: 80px; border-radius: 50%; object-fit: cover; position: absolute; right: 0; top: -10px; border: 2px solid #ddd; }
.section { margin-bottom: 25px; }
.section h3 { font-size: 18px; margin-bottom: 12px; padding-bottom: 5px; border-bottom: 1px solid #ddd; }
.summary-section { background: #f9fafb; padding: 15px; border-radius: 6px; border-left: 3px solid #409eff; }
.summary-text { color: #555; font-size: 14px; line-height: 1.8; }
.item { margin-bottom: 15px; }
.item-header { display: flex; justify-content: space-between; margin-bottom: 5px; }
.item p { color: #555; font-size: 14px; line-height: 1.6; }
.description { color: #666 !important; margin-top: 5px; }
.award { color: #e6a23c; font-weight: bold; }
.skill-tags { display: flex; flex-wrap: wrap; gap: 8px; }
.skill-tag { background: #e8f4fd; color: #2c7be5; padding: 4px 12px; border-radius: 15px; font-size: 13px; }
.avatar-upload { display: flex; align-items: center; gap: 15px; margin-bottom: 10px; }
.avatar-preview { width: 60px; height: 60px; border-radius: 50%; object-fit: cover; border: 2px solid #ddd; }
.sort-tip { font-size: 12px; color: #999; margin-bottom: 8px; }
.sort-list { list-style: none; }
.sort-item { display: flex; align-items: center; gap: 10px; padding: 8px 10px; margin-bottom: 5px; background: #fafafa; border-radius: 6px; border: 1px solid #eee; cursor: move; }
.sort-item:hover { background: #f0f2f5; }
.drag-handle { color: #999; font-size: 16px; cursor: grab; }
</style>