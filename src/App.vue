<template>
  <div id="app" :class="{ 'dark-mode': isDarkMode }">
    <!-- Navigation Bar -->
    <nav class="navbar navbar-expand-lg navbar-dark fixed-top" :class="navbarClass">
      <div class="container">
        <a class="navbar-brand fw-bold" href="#">
          <i class="bi bi-shield-lock-fill me-2"></i>
          سازنده پسورد پیشرفته
        </a>
        <div class="navbar-nav ms-auto">
          <button @click="toggleDarkMode" class="btn btn-outline-light btn-sm me-2">
            <i :class="isDarkMode ? 'bi bi-sun-fill' : 'bi bi-moon-fill'"></i>
          </button>
          <button @click="showInfo" class="btn btn-outline-light btn-sm">
            <i class="bi bi-info-circle"></i>
          </button>
        </div>
      </div>
    </nav>

    <!-- Main Content -->
    <div class="main-content">
      <!-- Hero Section -->
      <section class="hero-section text-center py-5" data-aos="fade-down">
        <div class="container">
          <div class="row justify-content-center">
            <div class="col-lg-8">
              <h1 class="display-4 fw-bold mb-3 text-gradient">
                <i class="bi bi-shield-lock-fill me-3"></i>
                سازنده پسورد پیشرفته
              </h1>
              <p class="lead mb-4">پسوردهای امن و قوی با الگوریتم‌های پیشرفته</p>
              <!-- Quick Stats -->
              <div class="row g-3 mb-5">
                <div class="col-md-3" data-aos="fade-up" data-aos-delay="100">
                  <div class="stat-card">
                    <i class="bi bi-lightning-charge-fill text-warning"></i>
                    <h4>{{ totalGenerated }}</h4>
                    <p>پسورد تولید شده</p>
                  </div>
                </div>
                <div class="col-md-3" data-aos="fade-up" data-aos-delay="200">
                  <div class="stat-card">
                    <i class="bi bi-shield-check text-success"></i>
                    <h4>{{ strongPasswords }}</h4>
                    <p>پسورد قوی</p>
                  </div>
                </div>
                <div class="col-md-3" data-aos="fade-up" data-aos-delay="300">
                  <div class="stat-card">
                    <i class="bi bi-clock-history text-info"></i>
                    <h4>{{ averageLength }}</h4>
                    <p>میانگین طول</p>
                  </div>
                </div>
                <div class="col-md-3" data-aos="fade-up" data-aos-delay="400">
                  <div class="stat-card">
                    <i class="bi bi-graph-up text-primary"></i>
                    <h4>{{ strengthScore }}</h4>
                    <p>امتیاز قدرت</p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Password Generator Card -->
          <div class="row justify-content-center">
            <div class="col-lg-7">
              <div class="card shadow-lg rounded-4 p-4 mb-4 glass-card" data-aos="zoom-in">
                <div class="card-body">
                  <!-- Password Display -->
                  <div class="input-group mb-3">
                    <input
                      type="text"
                      class="form-control form-control-lg text-center fw-bold"
                      :value="generatedPassword"
                      readonly
                      ref="passwordInput"
                      :class="{ 'is-invalid': generatedPassword === 'لطفاً حداقل یک گزینه را انتخاب کنید' }"
                    />
                    <button
                      class="btn btn-primary btn-lg"
                      type="button"
                      @click="copyPassword"
                      :disabled="generatedPassword === 'لطفاً حداقل یک گزینه را انتخاب کنید'"
                    >
                      <i :class="copied ? 'bi bi-clipboard-check-fill' : 'bi bi-clipboard'"></i>
                      {{ copied ? 'کپی شد!' : 'کپی' }}
                    </button>
                  </div>
                  <div v-if="generatedPassword === 'لطفاً حداقل یک گزینه را انتخاب کنید'" class="text-danger small mb-2">
                    لطفاً حداقل یک گزینه را انتخاب کنید.
                  </div>

                  <!-- Options -->
                  <div class="row g-3 mb-3">
                    <div class="col-12">
                      <label class="form-label fw-bold">طول پسورد: <span class="badge bg-secondary">{{ passwordLength }}</span></label>
                      <input
                        type="range"
                        class="form-range"
                        min="8"
                        max="64"
                        v-model="passwordLength"
                        @input="generatePassword"
                      />
                    </div>
                    <div class="col-6 col-md-4" v-for="option in options" :key="option.key">
                      <div class="form-check form-switch">
                        <input
                          class="form-check-input"
                          type="checkbox"
                          :id="option.key"
                          v-model="option.model"
                          @change="generatePassword"
                        />
                        <label class="form-check-label" :for="option.key">
                          <i :class="option.icon" class="me-1"></i>
                          {{ option.label }}
                        </label>
                      </div>
                    </div>
                  </div>

                  <!-- Generate Buttons -->
                  <div class="d-flex gap-2 mb-3">
                    <button class="btn btn-success flex-fill" @click="generatePassword">
                      <i class="bi bi-arrow-repeat me-1"></i>
                      تولید پسورد جدید
                    </button>
                    <button class="btn btn-outline-info flex-fill" @click="generateMultiple">
                      <i class="bi bi-list-ol me-1"></i>
                      تولید چندین پسورد
                    </button>
                  </div>

                  <!-- Strength Meter -->
                  <div v-if="passwordStrength" class="mb-3">
                    <div class="d-flex align-items-center mb-1">
                      <span class="fw-bold me-2">قدرت پسورد:</span>
                      <span :class="['badge', strengthBadgeClass]">{{ passwordStrength }}</span>
                    </div>
                    <div class="progress" style="height: 8px;">
                      <div
                        class="progress-bar"
                        :class="strengthBarClass"
                        role="progressbar"
                        :style="{ width: strengthPercentage + '%' }"
                        :aria-valuenow="strengthPercentage"
                        aria-valuemin="0"
                        aria-valuemax="100"
                      ></div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Multiple Passwords List -->
          <div v-if="multiplePasswords.length > 0" class="row justify-content-center" data-aos="fade-up">
            <div class="col-lg-7">
              <div class="card shadow rounded-4 p-3 mb-4 glass-card">
                <div class="card-body">
                  <h5 class="fw-bold mb-3"><i class="bi bi-list-ol me-2"></i>پسوردهای تولید شده:</h5>
                  <div class="list-group mb-3">
                    <div
                      v-for="(password, index) in multiplePasswords"
                      :key="index"
                      class="list-group-item d-flex justify-content-between align-items-center"
                    >
                      <span class="font-monospace">{{ password }}</span>
                      <button class="btn btn-outline-primary btn-sm" @click="copySpecificPassword(password)">
                        <i class="bi bi-clipboard"></i> کپی
                      </button>
                    </div>
                  </div>
                  <button class="btn btn-danger w-100" @click="clearMultiplePasswords">
                    <i class="bi bi-trash"></i> پاک کردن لیست
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>

    <!-- Footer -->
    <footer class="footer text-center py-4 mt-auto" :class="footerClass">
      <div class="container">
        <p class="mb-1">
          نوشته شده با
          <span style="color: #e25555; font-size: 1.2em;">❤️</span>
          توسط
          <a href="https://mohammadmajidiankhorasani.netlify.app/" target="_blank" class="fw-bold text-decoration-underline">
            محمد حسن مجیدیان
          </a>
        </p>
        <small class="text-muted">تمامی پسوردها فقط در مرورگر شما تولید می‌شوند و ذخیره نمی‌شوند.</small>
      </div>
    </footer>
  </div>
</template>

<script>
import Swal from 'sweetalert2'

export default {
  name: 'App',
  data() {
    return {
      generatedPassword: '',
      passwordLength: 16,
      includeUppercase: true,
      includeLowercase: true,
      includeNumbers: true,
      includeSymbols: true,
      excludeSimilar: false,
      copied: false,
      multiplePasswords: [],
      passwordStrength: '',
      strengthPercentage: 0,
      isDarkMode: false,
      totalGenerated: 0,
      strongPasswords: 0,
      averageLength: 0,
      strengthScore: 0
    }
  },
  computed: {
    options() {
      return [
        {
          key: 'uppercase',
          label: 'حروف بزرگ (A-Z)',
          icon: 'bi bi-type-uppercase',
          model: this.includeUppercase
        },
        {
          key: 'lowercase',
          label: 'حروف کوچک (a-z)',
          icon: 'bi bi-type-lowercase',
          model: this.includeLowercase
        },
        {
          key: 'numbers',
          label: 'اعداد (0-9)',
          icon: 'bi bi-123',
          model: this.includeNumbers
        },
        {
          key: 'symbols',
          label: 'نمادها (!@#$%^&*)',
          icon: 'bi bi-asterisk',
          model: this.includeSymbols
        },
        {
          key: 'excludeSimilar',
          label: 'حذف کاراکترهای مشابه (l, 1, I, O, 0)',
          icon: 'bi bi-slash-circle',
          model: this.excludeSimilar
        }
      ]
    },
    strengthBadgeClass() {
      if (this.passwordStrength === 'ضعیف') return 'bg-danger'
      if (this.passwordStrength === 'متوسط') return 'bg-warning text-dark'
      if (this.passwordStrength === 'قوی') return 'bg-success'
      if (this.passwordStrength === 'خیلی قوی') return 'bg-primary'
      return 'bg-secondary'
    },
    strengthBarClass() {
      if (this.passwordStrength === 'ضعیف') return 'bg-danger'
      if (this.passwordStrength === 'متوسط') return 'bg-warning'
      if (this.passwordStrength === 'قوی') return 'bg-success'
      if (this.passwordStrength === 'خیلی قوی') return 'bg-primary'
      return 'bg-secondary'
    },
    navbarClass() {
      return this.isDarkMode ? 'bg-dark' : 'bg-primary'
    },
    footerClass() {
      return this.isDarkMode ? 'bg-dark text-light' : 'bg-primary text-light'
    }
  },
  mounted() {
    this.loadTheme()
    this.generatePassword()
  },
  methods: {
    generatePassword() {
      const uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
      const lowercase = 'abcdefghijklmnopqrstuvwxyz'
      const numbers = '0123456789'
      const symbols = '!@#$%^&*()_+-=[]{}|;:,.<>?'

      let chars = ''
      if (this.includeUppercase) chars += uppercase
      if (this.includeLowercase) chars += lowercase
      if (this.includeNumbers) chars += numbers
      if (this.includeSymbols) chars += symbols

      if (chars === '') {
        this.generatedPassword = 'لطفاً حداقل یک گزینه را انتخاب کنید'
        return
      }

      if (this.excludeSimilar) {
        chars = chars.replace(/[l1IO0]/g, '')
      }

      let password = ''
      for (let i = 0; i < this.passwordLength; i++) {
        password += chars.charAt(Math.floor(Math.random() * chars.length))
      }

      this.generatedPassword = password
      this.copied = false
      this.totalGenerated++
      this.updateStats(password)
      this.calculatePasswordStrength()
    },

    generateMultiple() {
      this.multiplePasswords = []
      let strongCount = 0
      let totalLen = 0
      let totalScore = 0
      for (let i = 0; i < 5; i++) {
        const password = this.createPassword()
        this.multiplePasswords.push(password)
        totalLen += password.length
        const score = this.getStrengthScore(password)
        totalScore += score
        if (score >= 5) strongCount++
      }
      this.strongPasswords = strongCount
      this.averageLength = Math.round(totalLen / 5)
      this.strengthScore = Math.round(totalScore / 5)
    },

    createPassword() {
      const uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
      const lowercase = 'abcdefghijklmnopqrstuvwxyz'
      const numbers = '0123456789'
      const symbols = '!@#$%^&*()_+-=[]{}|;:,.<>?'

      let chars = ''
      if (this.includeUppercase) chars += uppercase
      if (this.includeLowercase) chars += lowercase
      if (this.includeNumbers) chars += numbers
      if (this.includeSymbols) chars += symbols

      if (this.excludeSimilar) {
        chars = chars.replace(/[l1IO0]/g, '')
      }

      let password = ''
      for (let i = 0; i < this.passwordLength; i++) {
        password += chars.charAt(Math.floor(Math.random() * chars.length))
      }
      return password
    },

    copyPassword() {
      if (this.generatedPassword && this.generatedPassword !== 'لطفاً حداقل یک گزینه را انتخاب کنید') {
        navigator.clipboard.writeText(this.generatedPassword).then(() => {
          this.copied = true
          setTimeout(() => {
            this.copied = false
          }, 2000)
        })
      }
    },

    copySpecificPassword(password) {
      navigator.clipboard.writeText(password).then(() => {
        Swal.fire({
          toast: true,
          position: 'top-end',
          icon: 'success',
          title: 'کپی شد!',
          showConfirmButton: false,
          timer: 1200,
          timerProgressBar: true
        })
      })
    },

    clearMultiplePasswords() {
      this.multiplePasswords = []
    },

    calculatePasswordStrength() {
      const password = this.generatedPassword
      const score = this.getStrengthScore(password)
      if (score <= 2) {
        this.passwordStrength = 'ضعیف'
        this.strengthPercentage = 25
      } else if (score <= 4) {
        this.passwordStrength = 'متوسط'
        this.strengthPercentage = 50
      } else if (score <= 6) {
        this.passwordStrength = 'قوی'
        this.strengthPercentage = 75
      } else {
        this.passwordStrength = 'خیلی قوی'
        this.strengthPercentage = 100
      }
    },

    getStrengthScore(password) {
      let score = 0
      if (password.length >= 8) score += 1
      if (password.length >= 12) score += 1
      if (password.length >= 16) score += 1
      if (/[a-z]/.test(password)) score += 1
      if (/[A-Z]/.test(password)) score += 1
      if (/[0-9]/.test(password)) score += 1
      if (/[^A-Za-z0-9]/.test(password)) score += 1
      if (password.length >= 20) score += 1
      return score
    },

    updateStats(password) {
      const score = this.getStrengthScore(password)
      if (score >= 5) this.strongPasswords++
      this.averageLength = password.length
      this.strengthScore = score
    },

    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode
      localStorage.setItem('passgen_dark', this.isDarkMode ? '1' : '0')
      document.body.classList.toggle('dark-bg', this.isDarkMode)
    },

    loadTheme() {
      const dark = localStorage.getItem('passgen_dark')
      this.isDarkMode = dark === '1'
      if (this.isDarkMode) {
        document.body.classList.add('dark-bg')
      }
    },

    showInfo() {
      Swal.fire({
        title: 'درباره برنامه',
        html: `
          <b>سازنده پسورد پیشرفته</b><br>
          <ul class="text-end" style="direction:rtl;">
            <li>تولید پسورد امن و قوی</li>
            <li>قابلیت تغییر تم (روشن/تاریک)</li>
            <li>انیمیشن‌های جذاب و رابط کاربری مدرن</li>
            <li>کاملاً آفلاین و PWA</li>
            <li>ساخته شده با ❤️ توسط <a href='https://mohammadmajidiankhorasani.netlify.app/' target='_blank'>محمد حسن مجیدیان</a></li>
          </ul>
        `,
        icon: 'info',
        confirmButtonText: 'بستن',
        customClass: {
          popup: 'text-end'
        }
      })
    }
  }
}
</script>

<style>
/* Glassmorphism Card */
.glass-card {
  background: rgba(255,255,255,0.7);
  border: 1px solid rgba(255,255,255,0.3);
  box-shadow: 0 8px 32px 0 rgba(31,38,135,0.15);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border-radius: 1.5rem;
  transition: background 0.3s;
}
.dark-mode .glass-card {
  background: rgba(30, 41, 59, 0.85);
  color: #f1f5f9;
  border: 1px solid rgba(51,65,85,0.3);
}
.text-gradient {
  background: linear-gradient(90deg, #0ea5e9 0%, #6366f1 100%);
  -webkit-text-fill-color: transparent;
}
.stat-card {
  background: rgba(255,255,255,0.8);
  border-radius: 1rem;
  padding: 1rem 0.5rem;
  box-shadow: 0 2px 8px rgba(0,0,0,0.07);
  margin-bottom: 0.5rem;
  transition: background 0.3s;
}
.stat-card i {
  font-size: 2rem;
  margin-bottom: 0.5rem;
  display: block;
}
.dark-mode .stat-card {
  background: rgba(30,41,59,0.9);
  color: #f1f5f9;
}
body.dark-bg {
  background: linear-gradient(135deg, #0a2540 0%, #1e3a8a 100%) !important;
}
.dark-mode {
  background: #0a2540;
  color: #f1f5f9;
}
.main-content {
  padding-top: 80px;
  min-height: 80vh;
}
.footer {
  font-size: 1.1rem;
  letter-spacing: 0.01em;
}
a {
  color: #0ea5e9;
}
a:hover {
  color: #6366f1;
}
</style>
