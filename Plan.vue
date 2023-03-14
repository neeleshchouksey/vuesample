<template>
  <section class="employee-section pettern-bg">
    <div class="header">

      <nav class="navbar navbar-expand-lg navbar-light ">
        <router-link class="navbar-brand" to="/employee/dashboard"><img src="../assets/images/logo.png">
        </router-link>

        <button class="navbar-toggler" data-bs-target="#navbarSupportedContent">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
                    <ul class="navbar-nav ml-auto cutom-menu-list">
                        <li class="nav-item">
                            <router-link class="nav-link btn custom-btn" to="/login">
                                <div class="gradient-btn">Login</div>
                            </router-link>
                        </li>
                    </ul>
                </div>
      </nav>
    </div>
    <div class="container">
      <div class="row">
        <section class="pricing-section">
          <div class="container">
            <h2 class="page-title">Pricing and <span>Plans</span></h2>
            <div class="nav  nav-pills me-3" id="v-pills-tab" role="tablist" aria-orientation="vertical">
              <button class="nav-link active" id="v-pills-home-tab" data-bs-toggle="pill" data-bs-target="#One-Time"
                type="button" role="tab" aria-controls="v-pills-home" aria-selected="true">One-Time (30-day
                access)
              </button>
              <button class="nav-link" id="v-pills-profile-tab" data-bs-toggle="pill"
                data-bs-target="#Subscriptions-plans" type="button" role="tab" aria-controls="v-pills-profile"
                aria-selected="false">Subscriptions plans
              </button>
            </div>
            <div class="tab-content" id="v-pills-tabContent">
              <div class="tab-pane fade show active" id="One-Time" role="tabpanel" aria-labelledby="One-Time">
                <div class="row">
                  <div class="col-md-6">
                    <h3 class="section-title">Plan Features</h3>
                  </div>
                  <!-- <div class="col-md-6 d-flex flex-wrap">
                    <div class="custom-radio-btn ml-auto mb-3">
                      <input type="radio" id="Monthly" name="radio-group">
                      <label for="Monthly">Monthly</label>
                    </div>
                    <div class="custom-radio-btn ml-4  mb-3">
                      <input type="radio" id="Yearly" name="radio-group" checked>
                      <label for="Yearly">Yearly</label>
                    </div>
                  </div> -->
                </div>
                <div class="row">
                  <div class="col-md-12">
                    <div class="pricing-box">
                      <div class="row">
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Assessment Frequency</p>
                            <p>1x</p>
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">24x7 Access to Employee Platform</p>
                            <img src="../assets/images/check.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Assessment Customization</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Guided Leader Transformation Journey</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Individual Report</p>
                            <img src="../assets/images/check.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Profile Based Employee Learning Journey</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Organizational Report and Management Metrics Dashboard</p>
                            <img src="../assets/images/check.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Resources</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Onboarding Support</p>
                            <img src="../assets/images/check.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Custom Workshops (Yearly)</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Custom Report</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Attend Live Workshops (Per Year Member)</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Dedicated Organizational Consulting( Yearly)</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Team Announcements Messaging</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Cognitive Dynamism Platform Features</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">Team Announcements Messaging</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-6">
                          <div class="pricing-text">
                            <p class="w-100">24x7 Access to Employer Platform</p>
                            <img src="../assets/images/cross.svg" alt="check icon" />
                          </div>
                        </div>
                        <div class="col-md-12">
                          <div class="action-btn">
                            <router-link class="btn-primary btn"
                              to="/create-company?plan=One-Time-Users-Plan-USD-Monthly&periodUnit=month&planType=One-Time-Users-Plan">
                              Get
                              Started
                            </router-link>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="tab-pane fade" id="Subscriptions-plans" role="tabpanel" aria-labelledby="Subscriptions-plans">
                <div class="row">
                  <div class="col-md-6">
                    <h3 class="section-title">Plans Comparison</h3>
                  </div>
                  <div class="col-md-6 d-flex flex-wrap plan-box">
                    <div class="custom-radio-btn ml-auto mb-3">
                      <input type="radio" id="Monthly1" name="radio-group1" v-on:click="showMonthly = true" checked>
                      <label for="Monthly1">Quarterly</label>
                    </div>
                    <div class="custom-radio-btn ml-4  mb-3">
                      <input type="radio" id="Yearly1" name="radio-group1" v-on:click="showMonthly = false">
                      <label for="Yearly1">Yearly</label>
                    </div>
                  </div>
                </div>
                <div class="desktop-plans">
                <div class="row monthly" v-if="showMonthly">
                  <div class="col-md-6">
                    <div class="pricing-box comparison-box">
                      <div class="row">
                        <div class="col-md-12">
                          <div class="pricing-title pink-bg">
                            <p class="w-100">Plan Features</p>
                          </div>
                        </div>
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Assessment Customization</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Assessment Frequency</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Individual Report</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Organizational Report and Management Metrics Dashboard</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Onboarding Support</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Custom Report</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Dedicated Organizational Consulting( Yearly)</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Cognitive Dynamism Platform Features</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">24x7 Access to Employer Platform</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">24x7 Access to Employee Platform</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Guided Leader Transformation Journey</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Profile Based Employee Learning Journey</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Resources</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Custom Workshops (Yearly)</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Attend Live Workshops (Per Year Member)</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Team Announcements Messaging</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Team Check-in and Pop-up Survey</p>
                          </div>
                        </div> <!-- list end -->
                      </div>
                    </div>
                  </div>
                  <div class="col-md-2">
                    <div class="pricing-box comparison-box text-center">
                      <div class="row">
                        <div class="col-md-12">
                          <div class="pricing-title yello-bg">
                            <p class="w-100 text-center">Gold</p>
                          </div>
                        </div>
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">1x</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">8 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">4 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-btn">
                            <router-link class="btn-primary btn"
                              to="/create-company?plan=Package-1-Basic-USD-Every-3-months&periodUnit=month&planType=Package-1-Basic">
                              Get
                              Started
                            </router-link>
                          </div>
                        </div> <!-- list end -->
                      </div>
                    </div>
                  </div>
                  <div class="col-md-2">
                    <div class="pricing-box comparison-box text-center">
                      <div class="row">
                        <div class="col-md-12">
                          <div class="pricing-title silver-bg">
                            <p class="w-100 text-center">Diamond</p>
                          </div>
                        </div>
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">4x</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">16 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">2 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">6 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-btn">
                            <router-link class="btn-primary btn"
                              to="/create-company?plan=Package-2-Enhanced-USD-Every-3-months&periodUnit=month&planType=Package-2-Enhanced">
                              Get
                              Started
                            </router-link>
                          </div>
                        </div> <!-- list end -->
                      </div>
                    </div>
                  </div>
                  <div class="col-md-2">
                    <div class="pricing-box comparison-box text-center">
                      <div class="row">
                        <div class="col-md-12">
                          <div class="pricing-title navi-blue-bg">
                            <p class="w-100 text-center">Platinum</p>
                          </div>
                        </div>
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">12x</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <!-- <p class="w-100">-</p> -->
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">96 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">8 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">12 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-btn">
                            <router-link class="btn-primary btn"
                              to="/create-company?plan=Package-3-Premier-USD-Every-3-months&periodUnit=month&planType=Package-3-Premier">
                              Get
                              Started
                            </router-link>
                            <!-- <a href="#" class="btn-primary btn">Get Started</a> -->
                          </div>
                        </div> <!-- list end -->
                      </div>
                    </div>
                  </div>
                </div>
                <div class="row yearly" v-if="!showMonthly">
                  <div class="col-md-6">
                    <div class="pricing-box comparison-box">
                      <div class="row">
                        <div class="col-md-12">
                          <div class="pricing-title pink-bg">
                            <p class="w-100">Plan Features</p>
                          </div>
                        </div>
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Assessment Customization</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Assessment Frequency</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Individual Report</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Organizational Report and Management Metrics Dashboard</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Onboarding Support</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Custom Report</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Dedicated Organizational Consulting( Yearly)</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Cognitive Dynamism Platform Features</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">24x7 Access to Employer Platform</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">24x7 Access to Employee Platform</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Guided Leader Transformation Journey</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Profile Based Employee Learning Journey</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Resources</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Custom Workshops (Yearly)</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Attend Live Workshops (Per Year Member)</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Team Announcements Messaging</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">Team Check-in and Pop-up Survey</p>
                          </div>
                        </div> <!-- list end -->
                      </div>
                    </div>
                  </div>
                  <div class="col-md-2">
                    <div class="pricing-box comparison-box text-center">
                      <div class="row">
                        <div class="col-md-12">
                          <div class="pricing-title yello-bg">
                            <p class="w-100 text-center">Gold</p>
                          </div>
                        </div>
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">1x</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">8 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">4 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-btn">
                            <router-link class="btn-primary btn"
                              to="/create-company?plan=Package-1-Basic-USD-Yearly&periodUnit=year&planType=Package-1-Basic">
                              Get
                              Started
                            </router-link>
                          </div>
                        </div> <!-- list end -->
                      </div>
                    </div>
                  </div>
                  <div class="col-md-2">
                    <div class="pricing-box comparison-box text-center">
                      <div class="row">
                        <div class="col-md-12">
                          <div class="pricing-title silver-bg">
                            <p class="w-100 text-center">Diamond</p>
                          </div>
                        </div>
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">4x</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">16 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">2 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">6 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-btn">
                            <router-link class="btn-primary btn"
                              to="/create-company?plan=Package-2-Enhanced-USD-Yearly&periodUnit=year&planType=Package-2-Enhanced">
                              Get
                              Started
                            </router-link>
                          </div>
                        </div> <!-- list end -->
                      </div>
                    </div>
                  </div>
                  <div class="col-md-2">
                    <div class="pricing-box comparison-box text-center">
                      <div class="row">
                        <div class="col-md-12">
                          <div class="pricing-title navi-blue-bg">
                            <p class="w-100 text-center">Platinum</p>
                          </div>
                        </div>
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">1x</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">8 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">-</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">8 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <p class="w-100">12 hours</p>
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-text">
                            <img src="../assets/images/check.svg" alt="check icon">
                          </div>
                        </div> <!-- list end -->
                        <div class="col-md-12">
                          <!-- list start -->
                          <div class="pricing-btn">
                            <router-link class="btn-primary btn"
                              to="/create-company?plan=Package-3-Premier-USD-Yearly&periodUnit=year&planType=Package-3-Premier">
                              Get
                              Started
                            </router-link>
                            <!-- <a href="#" class="btn-primary btn">Get Started</a> -->
                          </div>
                        </div> <!-- list end -->
                      </div>
                    </div>
                  </div>
                </div>
              </div>
                <div class="mobile-plans">
                  <VueSlickCarousel class="plan-card row monthly" v-if="showMonthly"  v-bind=" settings_shopping">
                    <div class="col-md-6">
                      <div class="pricing-box comparison-box">
                        <div class="row">
                          <div class="col-md-12">
                            <div class="pricing-title pink-bg">
                              <p class="w-100">Plan Features</p>
                            </div>
                          </div>
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Assessment Customization</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Assessment Frequency</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Individual Report</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Organizational Report and Management Metrics Dashboard</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Onboarding Support</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Custom Report</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Dedicated Organizational Consulting( Yearly)</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Cognitive Dynamism Platform Features</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">24x7 Access to Employer Platform</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">24x7 Access to Employee Platform</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Guided Leader Transformation Journey</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Profile Based Employee Learning Journey</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Resources</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Custom Workshops (Yearly)</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Attend Live Workshops (Per Year Member)</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Team Announcements Messaging</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Team Check-in and Pop-up Survey</p>
                            </div>
                          </div> <!-- list end -->
                        </div>
                      </div>
                    </div>
                    <div class="col-md-2">
                      <div class="pricing-box comparison-box text-center">
                        <div class="row">
                          <div class="col-md-12">
                            <div class="pricing-title yello-bg">
                              <p class="w-100 text-center">Gold</p>
                            </div>
                          </div>
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">1x</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">8 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">4 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-btn">
                              <router-link class="btn-primary btn"
                                to="/create-company?plan=Package-1-Basic-USD-Every-3-months&periodUnit=month&planType=Package-1-Basic">
                                Get
                                Started
                              </router-link>
                            </div>
                          </div> <!-- list end -->
                        </div>
                      </div>
                    </div>
                    <div class="col-md-2">
                      <div class="pricing-box comparison-box text-center">
                        <div class="row">
                          <div class="col-md-12">
                            <div class="pricing-title silver-bg">
                              <p class="w-100 text-center">Diamond</p>
                            </div>
                          </div>
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">4x</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">16 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">2 hours (Virtual)</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">6 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-btn">
                              <router-link class="btn-primary btn"
                                to="/create-company?plan=Package-2-Enhanced-USD-Every-3-months&periodUnit=month&planType=Package-2-Enhanced">
                                Get
                                Started
                              </router-link>
                            </div>
                          </div> <!-- list end -->
                        </div>
                      </div>
                    </div>
                    <div class="col-md-2">
                      <div class="pricing-box comparison-box text-center">
                        <div class="row">
                          <div class="col-md-12">
                            <div class="pricing-title navi-blue-bg">
                              <p class="w-100 text-center">Platinum</p>
                            </div>
                          </div>
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">12x</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <!-- <p class="w-100">-</p> -->
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">96 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">8 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">12 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-btn">
                              <router-link class="btn-primary btn"
                                to="/create-company?plan=Package-3-Premier-USD-Every-3-months&periodUnit=month&planType=Package-3-Premier">
                                Get
                                Started
                              </router-link>
                              <!-- <a href="#" class="btn-primary btn">Get Started</a> -->
                            </div>
                          </div> <!-- list end -->
                        </div>
                      </div>
                    </div>
                  </VueSlickCarousel>
                  <VueSlickCarousel class="plan-card row yearly" v-if="!showMonthly"  v-bind=" settings_shopping">
                    <div class="col-md-6">
                      <div class="pricing-box comparison-box">
                        <div class="row">
                          <div class="col-md-12">
                            <div class="pricing-title pink-bg">
                              <p class="w-100">Plan Features</p>
                            </div>
                          </div>
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Assessment Customization</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Assessment Frequency</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Individual Report</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Organizational Report and Management Metrics Dashboard</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Onboarding Support</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Custom Report</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Dedicated Organizational Consulting( Yearly)</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Cognitive Dynamism Platform Features</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">24x7 Access to Employer Platform</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">24x7 Access to Employee Platform</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Guided Leader Transformation Journey</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Profile Based Employee Learning Journey</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Resources</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Custom Workshops (Yearly)</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Attend Live Workshops (Per Year Member)</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Team Announcements Messaging</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">Team Check-in and Pop-up Survey</p>
                            </div>
                          </div> <!-- list end -->
                        </div>
                      </div>
                    </div>
                    <div class="col-md-2">
                      <div class="pricing-box comparison-box text-center">
                        <div class="row">
                          <div class="col-md-12">
                            <div class="pricing-title yello-bg">
                              <p class="w-100 text-center">Gold</p>
                            </div>
                          </div>
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">1x</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">8 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">4 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-btn">
                              <router-link class="btn-primary btn"
                                to="/create-company?plan=Package-1-Basic-USD-Yearly&periodUnit=year&planType=Package-1-Basic">
                                Get
                                Started
                              </router-link>
                            </div>
                          </div> <!-- list end -->
                        </div>
                      </div>
                    </div>
                    <div class="col-md-2">
                      <div class="pricing-box comparison-box text-center">
                        <div class="row">
                          <div class="col-md-12">
                            <div class="pricing-title silver-bg">
                              <p class="w-100 text-center">Diamond</p>
                            </div>
                          </div>
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">4x</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">16 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">2 hours (Virtual)</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">6 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-btn">
                              <router-link class="btn-primary btn"
                                to="/create-company?plan=Package-2-Enhanced-USD-Yearly&periodUnit=year&planType=Package-2-Enhanced">
                                Get
                                Started
                              </router-link>
                            </div>
                          </div> <!-- list end -->
                        </div>
                      </div>
                    </div>
                    <div class="col-md-2">
                      <div class="pricing-box comparison-box text-center">
                        <div class="row">
                          <div class="col-md-12">
                            <div class="pricing-title navi-blue-bg">
                              <p class="w-100 text-center">Platinum</p>
                            </div>
                          </div>
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">1x</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">8 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">-</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">8 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <p class="w-100">12 hours</p>
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-text">
                              <img src="../assets/images/check.svg" alt="check icon">
                            </div>
                          </div> <!-- list end -->
                          <div class="col-md-12">
                            <!-- list start -->
                            <div class="pricing-btn">
                              <router-link class="btn-primary btn"
                                to="/create-company?plan=Package-3-Premier-USD-Yearly&periodUnit=year&planType=Package-3-Premier">
                                Get
                                Started
                              </router-link>
                              <!-- <a href="#" class="btn-primary btn">Get Started</a> -->
                            </div>
                          </div> <!-- list end -->
                        </div>
                      </div>
                    </div>
                  </VueSlickCarousel>
              </div>
              </div>
            </div>
            <!--
                        <div class="row">
                          <div class="col-md-6 col-lg-4" v-for="p in plans" v-bind:key="p.id">
                            <div class="pricing-card card-color">
                              <div class="card-type">{{ p.name }}</div>
                              <div class="card-price">Features</div>
                              <div class="user-plan">
                                <div class="info-list">
                                  <ul class="list"> -->
            <!-- <li v-if="p.description" v-html="'->' +
              p.description.replace(/(?:\r\n|\r|\n)/g, '<br /> -> ')"></li> -->
            <!-- <li v-if="p.description" :inner-html.prop="p.description | replaceDescription"></li>
       </ul>
     </div>
   </div> -->
            <!-- <div class="info-list">
              <lu class="list">
                <li><i class="fa fa-check"></i>Store up to 20 songs</li>
                <li><i class="fa fa-check"></i>Store up to 20 songs</li>
                <li><i class="fa fa-check"></i>Store up to 20 songs</li>
                <li><i class="fa fa-check"></i>Store up to 20 songs</li>
                <li><i class="fa fa-check"></i>Store up to 20 songs</li>
              </lu>
            </div> -->
            <!--  <router-link class="btn-pricing"
               :to="'/create-company?plan=' + p.id + '&periodUnit=' + p.periodUnit + '&planType=' + p.itemId">Get
               Started
             </router-link>
           </div>
         </div>
       </div> -->
          </div>
        </section>
      </div>
    </div>
    <FooterEmployee></FooterEmployee>
  </section>
</template>

<script>
import Api from '../router/api'
import HeaderEmployee from '../components/employee/includes/Header'
import FooterEmployee from '../components/employee/includes/Footer'
import VueSlickCarousel from 'vue-slick-carousel'
import 'vue-slick-carousel/dist/vue-slick-carousel.css'
// optional style for arrows & dots
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'
/* eslint-disable */
export default {
  name: 'Plan',
  data() {
    return {
      msg: 'Plan',
      plans: [],
      showMonthly: true,
      //plan carousel
      settings_shopping: {
        centerMode: false,
        dots: true,
        infinite: true,
        speed: 500,
        controls: true,
        slidesToShow: 1,
        slidesToScroll: 1,
        initialSlide: 0,
        // adaptiveHeight: true,
        rtl: false,
      },
    }
  },
  
  components: { HeaderEmployee, FooterEmployee,  VueSlickCarousel },
  methods: {
    getPlans: function () {
      let that = this
      Api.getPlans().then(response => {
        that.plans = response.data.res
      }
      ).catch((error) => {

      })
    },
  },
  mounted() {
    this.getPlans()
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form-group {
  margin-bottom: 1rem;
}

.err {
  color: red;
}
</style>
