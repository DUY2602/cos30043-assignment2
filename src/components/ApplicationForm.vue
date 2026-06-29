<script setup>
import { ref, reactive, nextTick } from 'vue';

const form = reactive({
  firstName: '',
  lastName: '',
  username: '',
  password: '',
  confirmPassword: '',
  email: '',
  streetAddress: '',
  suburb: '',
  postcode: '',
  mobileNumber: '',
  dob: '',
  jobCategory: '',
  termsVisible: false,
});

const errors = reactive({});
const submitted = ref(false);
const formEl = ref(null);
const touched = reactive({});

function touchField(field) {
  touched[field] = true;
  validateField(field);
}

function validateField(field) {
  switch (field) {
    case 'firstName':
      if (!form.firstName || !lettersOnly.test(form.firstName)) {
        errors.firstName = 'First name is required and must contain only letters.';
      } else {
        errors.firstName = '';
      }
      break;
    case 'lastName':
      if (!form.lastName || !lettersOnly.test(form.lastName)) {
        errors.lastName = 'Last name is required and must contain only letters.';
      } else {
        errors.lastName = '';
      }
      break;
    case 'username':
      if (!form.username || form.username.length < 3) {
        errors.username = 'Username is required and must be at least 3 characters.';
      } else {
        errors.username = '';
      }
      break;
    case 'password':
      if (!form.password || form.password.length < 8) {
        errors.password = 'Password is required and must be at least 8 characters.';
      } else if (!/[$\%^\&\*]/.test(form.password)) {
        errors.password = 'Password must contain at least one special character ($, %, ^, &, *).';
      } else {
        errors.password = '';
      }
      break;
    case 'confirmPassword':
      if (form.confirmPassword !== form.password) {
        errors.confirmPassword = 'Passwords do not match.';
      } else {
        errors.confirmPassword = '';
      }
      break;
    case 'email':
      if (!form.email || !emailRegex.test(form.email)) {
        errors.email = 'A valid email address is required.';
      } else {
        errors.email = '';
      }
      break;
    case 'streetAddress':
      if (form.streetAddress && form.streetAddress.length > 40) {
        errors.streetAddress = 'Street address must be at most 40 characters.';
      } else {
        errors.streetAddress = '';
      }
      break;
    case 'suburb':
      if (form.suburb && form.suburb.length > 20) {
        errors.suburb = 'Suburb must be at most 20 characters.';
      } else {
        errors.suburb = '';
      }
      break;
    case 'postcode':
      if (!form.postcode || !fourDigits.test(form.postcode)) {
        errors.postcode = 'Postcode is required and must be exactly 4 digits.';
      } else {
        errors.postcode = '';
      }
      break;
    case 'mobileNumber':
      if (!form.mobileNumber || !startsWith04.test(form.mobileNumber)) {
        errors.mobileNumber = 'Mobile number is required and must be exactly 10 digits starting with 04.';
      } else {
        errors.mobileNumber = '';
      }
      break;
    case 'dob':
      if (!form.dob) {
        errors.dob = 'Date of birth is required.';
      } else {
        const today = new Date();
        const birth = new Date(form.dob);
        let age = today.getFullYear() - birth.getFullYear();
        const monthDiff = today.getMonth() - birth.getMonth();
        if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birth.getDate())) {
          age--;
        }
        if (age < 16) {
          errors.dob = 'Applicant must be at least 16 years old.';
        } else {
          errors.dob = '';
        }
      }
      break;
    case 'jobCategory':
      if (!form.jobCategory) {
        errors.jobCategory = 'Preferred job category is required.';
      } else {
        errors.jobCategory = '';
      }
      break;
  }
}

function validateForm() {
  let isValid = true;

  if (!form.firstName || !lettersOnly.test(form.firstName)) {
    errors.firstName = 'First name is required and must contain only letters.';
    isValid = false;
  }

  if (!form.lastName || !lettersOnly.test(form.lastName)) {
    errors.lastName = 'Last name is required and must contain only letters.';
    isValid = false;
  }

  if (!form.username || form.username.length < 3) {
    errors.username = 'Username is required and must be at least 3 characters.';
    isValid = false;
  }

  if (!form.password || form.password.length < 8) {
    errors.password = 'Password is required and must be at least 8 characters.';
    isValid = false;
  } else if (!/[$\%^\&\*]/.test(form.password)) {
    errors.password = 'Password must contain at least one special character ($, %, ^, &, *).';
    isValid = false;
  }

  if (form.confirmPassword !== form.password) {
    errors.confirmPassword = 'Passwords do not match.';
    isValid = false;
  }

  if (!form.email || !emailRegex.test(form.email)) {
    errors.email = 'A valid email address is required.';
    isValid = false;
  }

  if (form.streetAddress && form.streetAddress.length > 40) {
    errors.streetAddress = 'Street address must be at most 40 characters.';
    isValid = false;
  }

  if (form.suburb && form.suburb.length > 20) {
    errors.suburb = 'Suburb must be at most 20 characters.';
    isValid = false;
  }

  if (!form.postcode || !fourDigits.test(form.postcode)) {
    errors.postcode = 'Postcode is required and must be exactly 4 digits.';
    isValid = false;
  }

  if (!form.mobileNumber || !startsWith04.test(form.mobileNumber)) {
    errors.mobileNumber = 'Mobile number is required and must be exactly 10 digits starting with 04.';
    isValid = false;
  }

  if (!form.dob) {
    errors.dob = 'Date of birth is required.';
    isValid = false;
  } else {
    const today = new Date();
    const birth = new Date(form.dob);
    let age = today.getFullYear() - birth.getFullYear();
    const monthDiff = today.getMonth() - birth.getMonth();
    if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birth.getDate())) {
      age--;
    }
    if (age < 16) {
      errors.dob = 'Applicant must be at least 16 years old.';
      isValid = false;
    }
  }

  if (!form.jobCategory) {
    errors.jobCategory = 'Preferred job category is required.';
    isValid = false;
  }

  return isValid;
}

function onSubmit(event) {
  event.preventDefault();
  submitted.value = true;
  if (validateForm()) {
    nextTick(() => {
      if (formEl.value) {
        formEl.value.submit();
      }
    });
  }
}

</script>

<template>
  <div class="apply-container">
    <div class="text-center mb-4">
      <h2 class="fw-bold">Application Form</h2>
      <p class="text-muted">Fill in your details below to apply for a position.</p>
    </div>

    <div class="row justify-content-center">
      <div class="col-lg-10">
        <div class="card border-0 shadow-sm">
          <div class="card-body p-4 p-md-5">
            <form
              ref="formEl"
              method="post"
              action="http://mercury.swin.edu.au/it000000/formtest.php"
              novalidate
              @submit="onSubmit"
            >
              <fieldset class="mb-4 border rounded-3 p-3 bg-light">
                <legend class="fw-semibold px-2">Personal Details</legend>

                <div class="row g-3">
                  <div class="col-md-6">
                    <label for="firstName" class="form-label">First Name</label>
                    <input
                      type="text"
                      id="firstName"
                      name="firstName"
                      v-model="form.firstName"
                      class="form-control"
                      :class="{ 'is-invalid': touched.firstName && errors.firstName }"
                      placeholder="John"
                      @blur="touchField('firstName')"
                    >
                    <div class="invalid-feedback" v-if="touched.firstName && errors.firstName">{{ errors.firstName }}</div>
                  </div>

                  <div class="col-md-6">
                    <label for="lastName" class="form-label">Last Name</label>
                    <input
                      type="text"
                      id="lastName"
                      name="lastName"
                      v-model="form.lastName"
                      class="form-control"
                      :class="{ 'is-invalid': touched.lastName && errors.lastName }"
                      placeholder="Doe"
                      @blur="touchField('lastName')"
                    >
                    <div class="invalid-feedback" v-if="touched.lastName && errors.lastName">{{ errors.lastName }}</div>
                  </div>

                  <div class="col-md-6">
                    <label for="username" class="form-label">Username</label>
                    <input
                      type="text"
                      id="username"
                      name="username"
                      v-model="form.username"
                      class="form-control"
                      :class="{ 'is-invalid': touched.username && errors.username }"
                      placeholder="johndoe"
                      @blur="touchField('username')"
                    >
                    <div class="invalid-feedback" v-if="touched.username && errors.username">{{ errors.username }}</div>
                  </div>

                  <div class="col-md-6">
                    <label for="email" class="form-label">Email</label>
                    <input
                      type="email"
                      id="email"
                      name="email"
                      v-model="form.email"
                      class="form-control"
                      :class="{ 'is-invalid': touched.email && errors.email }"
                      placeholder="john@example.com"
                      @blur="touchField('email')"
                    >
                    <div class="invalid-feedback" v-if="touched.email && errors.email">{{ errors.email }}</div>
                  </div>

                  <div class="col-md-6">
                    <label for="password" class="form-label">Password</label>
                    <input
                      type="password"
                      id="password"
                      name="password"
                      v-model="form.password"
                      class="form-control"
                      :class="{ 'is-invalid': touched.password && errors.password }"
                      placeholder="Min. 8 characters"
                      @blur="touchField('password')"
                    >
                    <div class="invalid-feedback" v-if="touched.password && errors.password">{{ errors.password }}</div>
                  </div>

                  <div class="col-md-6">
                    <label for="confirmPassword" class="form-label">Confirm Password</label>
                    <input
                      type="password"
                      id="confirmPassword"
                      name="confirmPassword"
                      v-model="form.confirmPassword"
                      class="form-control"
                      :class="{ 'is-invalid': touched.confirmPassword && errors.confirmPassword }"
                      placeholder="Re-enter password"
                      @blur="touchField('confirmPassword')"
                    >
                    <div class="invalid-feedback" v-if="touched.confirmPassword && errors.confirmPassword">{{ errors.confirmPassword }}</div>
                  </div>
                </div>
              </fieldset>

              <fieldset class="mb-4 border rounded-3 p-3 bg-light">
                <legend class="fw-semibold px-2">Address & Contact</legend>

                <div class="row g-3">
                  <div class="col-12">
                    <label for="streetAddress" class="form-label">Street Address <span class="text-muted">(optional)</span></label>
                    <input
                      type="text"
                      id="streetAddress"
                      name="streetAddress"
                      v-model="form.streetAddress"
                      maxlength="40"
                      class="form-control"
                      :class="{ 'is-invalid': touched.streetAddress && errors.streetAddress }"
                      placeholder="123 Main St"
                      @blur="touchField('streetAddress')"
                    >
                    <div class="invalid-feedback" v-if="touched.streetAddress && errors.streetAddress">{{ errors.streetAddress }}</div>
                  </div>

                  <div class="col-md-5">
                    <label for="suburb" class="form-label">Suburb <span class="text-muted">(optional)</span></label>
                    <input
                      type="text"
                      id="suburb"
                      name="suburb"
                      v-model="form.suburb"
                      maxlength="20"
                      class="form-control"
                      :class="{ 'is-invalid': touched.suburb && errors.suburb }"
                      placeholder="Melbourne"
                      @blur="touchField('suburb')"
                    >
                    <div class="invalid-feedback" v-if="touched.suburb && errors.suburb">{{ errors.suburb }}</div>
                  </div>

                  <div class="col-md-3">
                    <label for="postcode" class="form-label">Postcode</label>
                    <input
                      type="text"
                      id="postcode"
                      name="postcode"
                      v-model="form.postcode"
                      maxlength="4"
                      class="form-control"
                      :class="{ 'is-invalid': touched.postcode && errors.postcode }"
                      placeholder="3000"
                      @blur="touchField('postcode')"
                    >
                    <div class="invalid-feedback" v-if="touched.postcode && errors.postcode">{{ errors.postcode }}</div>
                  </div>

                  <div class="col-md-4">
                    <label for="mobileNumber" class="form-label">Mobile Number</label>
                    <input
                      type="text"
                      id="mobileNumber"
                      name="mobileNumber"
                      v-model="form.mobileNumber"
                      maxlength="10"
                      class="form-control"
                      :class="{ 'is-invalid': touched.mobileNumber && errors.mobileNumber }"
                      placeholder="0412345678"
                      @blur="touchField('mobileNumber')"
                    >
                    <div class="invalid-feedback" v-if="touched.mobileNumber && errors.mobileNumber">{{ errors.mobileNumber }}</div>
                  </div>

                  <div class="col-md-4">
                    <label for="dob" class="form-label">Date of Birth</label>
                    <input
                      type="date"
                      id="dob"
                      name="dob"
                      v-model="form.dob"
                      class="form-control"
                      :class="{ 'is-invalid': touched.dob && errors.dob }"
                      @blur="touchField('dob')"
                    >
                    <div class="invalid-feedback" v-if="touched.dob && errors.dob">{{ errors.dob }}</div>
                  </div>

                  <div class="col-md-8">
                    <label for="jobCategory" class="form-label">Preferred Job Category</label>
                    <select
                      id="jobCategory"
                      name="jobCategory"
                      v-model="form.jobCategory"
                      class="form-select"
                      :class="{ 'is-invalid': touched.jobCategory && errors.jobCategory }"
                      @change="touchField('jobCategory')"
                    >
                      <option value="">Select a category...</option>
                      <option>AI</option>
                      <option>Data Science</option>
                      <option>Software Development</option>
                      <option>DevOps</option>
                      <option>Cybersecurity</option>
                      <option>Other</option>
                    </select>
                     <div class="invalid-feedback" v-if="touched.jobCategory && errors.jobCategory">{{ errors.jobCategory }}</div>
                  </div>
                </div>
              </fieldset>

              <fieldset class="mb-4 border rounded-3 p-3 bg-light">
                <legend class="fw-semibold px-2">Terms and Conditions</legend>
                <button
                  type="button"
                  class="btn btn-outline-secondary btn-sm"
                  @click="toggleTerms"
                >
                  {{ form.termsVisible ? 'Hide' : 'Show' }} Terms and Conditions
                </button>
                <div v-if="form.termsVisible" class="mt-3 p-3 bg-white border rounded-3 small text-muted lh-base">
                  <strong>Terms and Conditions for Application Entry:</strong><br><br>
                  1. First Name and Last Name must contain letters only.<br>
                  2. Username must be at least 3 characters long.<br>
                  3. Password must be at least 8 characters and include at least one special character ($, %, ^, &, *).<br>
                  4. Confirm Password must match the Password field.<br>
                  5. Email must be in a valid email format.<br>
                  6. Street Address is optional but must not exceed 40 characters.<br>
                  7. Suburb is optional but must not exceed 20 characters.<br>
                  8. Postcode is required and must be exactly 4 digits.<br>
                  9. Mobile Number is required, must be exactly 10 digits, and must start with 04.<br>
                  10. Date of Birth is required; applicant must be at least 16 years old.<br>
                  11. Preferred Job Category must be selected from the dropdown (AI, Data Science, Web Development, etc.).<br>
                  12. All required fields must be completed before submission.<br>
                  13. Providing false or misleading information may result in disqualification.
                </div>
              </fieldset>

              <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                <button type="submit" class="btn btn-primary px-5 py-2" :disabled="submitted">
                  <span v-if="!submitted">Submit Application</span>
                  <span v-else>
                    <span class="spinner-border spinner-border-sm me-1" role="status" aria-hidden="true"></span>
                    Submitted
                  </span>
                </button>
              </div>
            </form>

            <div v-if="submitted" class="mt-4 alert alert-success border-0 shadow-sm">
              <strong>Success!</strong> Thank you for your application. We will contact you shortly.
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.apply-container {
  animation: fadeIn 0.4s ease;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
.is-invalid {
  border-color: #dc3545 !important;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 12 12' width='12' height='12' fill='none' stroke='%23dc3545'%3e%3ccircle cx='6' cy='6' r='4.5'/%3e%3cpath stroke-linecap='round' d='M6 3.5v3M6 8.5h.01'/%3e%3c/svg%3e") !important;
}
</style>
