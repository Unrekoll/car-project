<template>
  <div id="app">
    <header class="header">
      <div class="container">
        <div class="container-header">
          <div class="logo">
            <img src="@/assets/logo.png" />
          </div>
          <div class="header-navbar">
            <p class="points">Inventory</p>
            <p class="points">Financing</p>
            <p class="points">Contacts</p>
            <img class="social" src="@/assets/Facebook.svg" />
            <img class="social" src="@/assets/Instagram.svg" />
          </div>
        </div>
        <div class="container-2">
          <h1 style="color: white">The right way to get a car</h1>
          <div class="descr-container">
            <p class="descr-container__descr-header">
              Monza Motorsports is a boutique-style dealership specializing in
              premium imports. We offer an advanced online vehicle purchasing
              experience with integrated financing and contactless home
              delivery.
            </p>
            <p class="descr-container__descr-header">
              All of our cars come with a 5 day/500 kilometres no-hassle
              exchange option. Since 2012 Monza Motorsports has been the
              benchmark for innovation in the automotive industry.
            </p>
          </div>
          <button type="button" class="button">Browse cars</button>
        </div>
      </div>
    </header>
    <div class="container cars-block">
      <div class="row gap-3">
        <div class="col-12">
          <h2 class="text-center text-uppercase fw-bold firstTitle">
            CURRENT OFFERS
          </h2>
          <div class="filter-bar">
            <div>
              <b-dropdown
                id="dropdown-1"
                :text="filterStatus.yearsFilterState"
                class="nav-item"
                :class="{
                  'filter-selected-state':
                    filterStatus.yearsFilterState !== 'Year',
                }"
              >
                <b-dropdown-item
                  v-for="(year, index) in yearsFilterOptions"
                  :class="{
                    'selected-filter-option':
                      filterStatus.yearsFilterState === year,
                  }"
                  :key="year"
                  :id="`year_${index}`"
                  @click="filterByYear(year)"
                >
                  {{ year }}
                </b-dropdown-item>
              </b-dropdown>
              <b-dropdown
                id="dropdown-2"
                :text="filterStatus.carFilterState"
                class="nav-item"
                :class="{
                  'filter-selected-state':
                    filterStatus.carFilterState !== 'Car',
                }"
              >
                <b-dropdown-item
                  v-for="(car, index) in carFilterOptions"
                  :class="{
                    'selected-filter-option':
                      filterStatus.carFilterState === car,
                  }"
                  :key="car"
                  :id="`car_${index}`"
                  @click="filterByCar(car)"
                >
                  {{ car }}
                </b-dropdown-item>
              </b-dropdown>
              <b-dropdown
                id="dropdown-3"
                :text="filterStatus.modelFilterState"
                class="nav-item"
                :class="{
                  'filter-selected-state':
                    filterStatus.modelFilterState !== 'Model',
                }"
              >
                <b-dropdown-item
                  v-for="(model, index) in modelFilterOptions"
                  :class="{
                    'selected-filter-option':
                      filterStatus.modelFilterState === model,
                  }"
                  :key="model"
                  :id="`model_${index}`"
                  @click="filterByModel(model)"
                >
                  {{ model }}
                </b-dropdown-item>
              </b-dropdown>
              <b-dropdown
                id="dropdown-4"
                :text="filterStatus.trimFilterState"
                class="nav-item"
                :class="{
                  'filter-selected-state':
                    filterStatus.trimFilterState !== 'Trim',
                }"
              >
                <b-dropdown-item
                  v-for="(trim, index) in trimFilterOptions"
                  :class="{
                    'selected-filter-option':
                      filterStatus.trimFilterState === trim,
                  }"
                  :key="trim"
                  :id="`trim_${index}`"
                  @click="filterByTrim(trim)"
                >
                  {{ trim }}
                </b-dropdown-item>
              </b-dropdown>
              <b-dropdown
                id="dropdown-5"
                :text="filterStatus.mileageFilterState"
                class="nav-item"
                :class="{
                  'filter-selected-state':
                    filterStatus.mileageFilterState !== 'Mileage',
                }"
              >
                <b-dropdown-item
                  v-for="(mileage, index) in mileageFilterOptions"
                  :class="{
                    'selected-filter-option':
                      filterStatus.mileageFilterState === mileage,
                  }"
                  :key="mileage"
                  :id="`mileage_${index}`"
                  @click="filterByMileage(mileage)"
                >
                  {{ mileage }}
                </b-dropdown-item>
              </b-dropdown>
            </div>
            <button type="button" class="button btn-cars">
              {{ filteredCarsCounter }}
            </button>
          </div>
          <div class="row cars-container">
            <div class="check">
              <div class="form-check">
                <input
                  class="form-check-input"
                  type="checkbox"
                  value=""
                  id="flexCheckDefault"
                />
                <label class="form-check-label" for="flexCheckDefault">
                  Instantly available vehicles
                </label>
              </div>
              <div class="sort">
                <h6 style="color: black">Sort by</h6>
                <img
                  class="img-sort"
                  src="@/assets/sort.svg"
                  alt=""
                  :class="{ ascending: isAscending }"
                />
                <b-dropdown
                  id="dropdown-5"
                  :text="sortSelectState"
                  class="nav-item sort-block"
                >
                  <b-dropdown-item
                    v-for="(sortOption, index) in sortOptions"
                    :class="{
                      'selected-filter-option': sortSelectState === sortOption,
                    }"
                    :key="sortOption"
                    :id="`sort_${index}`"
                    @click="sortBy(sortOption)"
                  >
                    {{ sortOption }}
                  </b-dropdown-item>
                </b-dropdown>
              </div>
            </div>
          </div>
        </div>
        <div class="grid-container col-12">
          <div class="car" v-for="(car, index) in carsTestData" :key="index">
            <img src="@/assets/car-1.png" class="carImage" />
            <div class="car-descr">
              <section class="aboutCar">
                <div class="year">{{ car.year }}</div>
                <h6 class="model fw-normal">{{ car.model }}</h6>
              </section>
              <section class="carInfo fw-light">
                <div class="mileage info">{{ car.mileage }}</div>
                <div class="vr"></div>
                <div class="make info">Automatic</div>
                <div class="vr"></div>
                <div class="trim info">Diesel</div>
              </section>
              <div class="price fw-bolder">{{ car.price }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="question-block d-flex">
      <div class="container d-flex align-items-center justify-content-between">
        <div class="subscribe-text">
          <h4 class="subs-title">Haven't found a suitable vehicle?</h4>
          <p class="subs-descr">
            Sign up for our newsletter and be the first to know when we publish
            new vehicle offers.
          </p>
        </div>
        <div class="subscribe d-flex">
          <div class="subscribe-input">
            <input
              type="text"
              class="form-control"
              placeholder="Enter your email"
              v-model="email"
              :class="[emailValidationStatus]"
            />
            <span
              v-show="emailValidationStatus === 'email-error'"
              class="errorMessage"
              >Email is incorrect</span
            >
            <b-icon-check2
              v-show="emailValidationStatus === 'email-success'"
              class="input-icon success-input-icon"
            ></b-icon-check2>
            <b-icon-x
              v-show="emailValidationStatus === 'email-error'"
              class="input-icon error-input-icon"
            ></b-icon-x>
          </div>
          <button
            :disabled="email === ''"
            class="button subsc-btn"
            @click="sendEmail"
          >
            Subscribe now
          </button>
        </div>
      </div>
    </div>
    <div class="section">
      <div class="container">
        <h2 class="text-center text-uppercase firstTitle mb-80">
          popular questions
        </h2>
        <div role="tablist">
          <b-card no-body class="mb-1 accordion">
            <b-card-header
              header-tag="header"
              class="accordion-header"
              role="tab"
              v-b-toggle.accordion-1
            >
              <h6>1. Scope</h6>
              <span class="when-open h4 mb-2"
                ><b-icon-dash class="accordion-dash-icon"></b-icon-dash
              ></span>
              <span class="when-closed h4 mb-2"
                ><b-icon-plus></b-icon-plus
              ></span>
            </b-card-header>
            <b-collapse
              id="accordion-1"
              visible
              accordion="my-accordion"
              role="tabpanel"
            >
              <b-card-body>
                <b-card-text class="accordion-text"
                  >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam
                  dignissim laoreet lobortis. Duis scelerisque, quam id ornare
                  tempus, nunc lorem blandit quam, vel euismod ligula tortor id
                  mi. Vestibulum vehicula placerat condimentum.</b-card-text
                >
              </b-card-body>
            </b-collapse>
          </b-card>
          <b-card no-body class="mb-1 accordion">
            <b-card-header
              header-tag="header"
              class="accordion-header"
              role="tab"
              v-b-toggle.accordion-2
            >
              <h6>2. Vehicle characteristics</h6>
              <span class="when-open h4 mb-2"
                ><b-icon-dash class="accordion-dash-icon"></b-icon-dash
              ></span>
              <span class="when-closed h4 mb-2"
                ><b-icon-plus></b-icon-plus
              ></span>
            </b-card-header>
            <b-collapse
              id="accordion-2"
              visible
              accordion="my-accordion"
              role="tabpanel"
            >
              <b-card-body>
                <b-card-text class="accordion-text"
                  >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam
                  dignissim laoreet lobortis. Duis scelerisque, quam id ornare
                  tempus, nunc lorem blandit quam, vel euismod ligula tortor id
                  mi. Vestibulum vehicula placerat condimentum.</b-card-text
                >
              </b-card-body>
            </b-collapse>
          </b-card>
          <b-card no-body class="mb-1 accordion">
            <b-card-header
              header-tag="header"
              class="accordion-header"
              role="tab"
              v-b-toggle.accordion-3
            >
              <h6>3. Warranty / guarantee</h6>
              <span class="when-open h4 mb-2"
                ><b-icon-dash class="accordion-dash-icon"></b-icon-dash
              ></span>
              <span class="when-closed h4 mb-2"
                ><b-icon-plus></b-icon-plus
              ></span>
            </b-card-header>
            <b-collapse
              id="accordion-3"
              visible
              accordion="my-accordion"
              role="tabpanel"
            >
              <b-card-body>
                <b-card-text class="accordion-text"
                  >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam
                  dignissim laoreet lobortis. Duis scelerisque, quam id ornare
                  tempus, nunc lorem blandit quam, vel euismod ligula tortor id
                  mi. Vestibulum vehicula placerat condimentum.</b-card-text
                >
              </b-card-body>
            </b-collapse>
          </b-card>
          <b-card no-body class="mb-1 accordion">
            <b-card-header
              header-tag="header"
              class="accordion-header"
              role="tab"
              v-b-toggle.accordion-4
            >
              <h6>4. Trade-in vehicle</h6>
              <span class="when-open h4 mb-2"
                ><b-icon-dash class="accordion-dash-icon"></b-icon-dash
              ></span>
              <span class="when-closed h4 mb-2"
                ><b-icon-plus></b-icon-plus
              ></span>
            </b-card-header>
            <b-collapse
              id="accordion-4"
              visible
              accordion="my-accordion"
              role="tabpanel"
            >
              <b-card-body>
                <b-card-text class="accordion-text"
                  >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam
                  dignissim laoreet lobortis. Duis scelerisque, quam id ornare
                  tempus, nunc lorem blandit quam, vel euismod ligula tortor id
                  mi. Vestibulum vehicula placerat condimentum.</b-card-text
                >
              </b-card-body>
            </b-collapse>
          </b-card>
          <b-card no-body class="mb-1 accordion">
            <b-card-header
              header-tag="header"
              class="accordion-header"
              role="tab"
              v-b-toggle.accordion-5
            >
              <h6>5. Home Delivery</h6>
              <span class="when-open h4 mb-2"
                ><b-icon-dash class="accordion-dash-icon"></b-icon-dash
              ></span>
              <span class="when-closed h4 mb-2"
                ><b-icon-plus></b-icon-plus
              ></span>
            </b-card-header>
            <b-collapse
              id="accordion-5"
              visible
              accordion="my-accordion"
              role="tabpanel"
            >
              <b-card-body>
                <b-card-text class="accordion-text"
                  >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam
                  dignissim laoreet lobortis. Duis scelerisque, quam id ornare
                  tempus, nunc lorem blandit quam, vel euismod ligula tortor id
                  mi. Vestibulum vehicula placerat condimentum.</b-card-text
                >
              </b-card-body>
            </b-collapse>
          </b-card>
          <b-card no-body class="mb-1 accordion">
            <b-card-header
              header-tag="header"
              class="accordion-header"
              role="tab"
              v-b-toggle.accordion-6
            >
              <h6>6. Right of return</h6>
              <span class="when-open h4 mb-2"
                ><b-icon-dash class="accordion-dash-icon"></b-icon-dash
              ></span>
              <span class="when-closed h4 mb-2"
                ><b-icon-plus></b-icon-plus
              ></span>
            </b-card-header>
            <b-collapse
              id="accordion-6"
              visible
              accordion="my-accordion"
              role="tabpanel"
            >
              <b-card-body>
                <b-card-text class="accordion-text"
                  >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam
                  dignissim laoreet lobortis. Duis scelerisque, quam id ornare
                  tempus, nunc lorem blandit quam, vel euismod ligula tortor id
                  mi. Vestibulum vehicula placerat condimentum.</b-card-text
                >
              </b-card-body>
            </b-collapse>
          </b-card>
          <b-card no-body class="mb-1 accordion">
            <b-card-header
              header-tag="header"
              class="accordion-header"
              role="tab"
              v-b-toggle.accordion-7
            >
              <h6>7. Written form</h6>
              <span class="when-open h4 mb-2"
                ><b-icon-dash class="accordion-dash-icon"></b-icon-dash
              ></span>
              <span class="when-closed h4 mb-2"
                ><b-icon-plus></b-icon-plus
              ></span>
            </b-card-header>
            <b-collapse
              id="accordion-7"
              visible
              accordion="my-accordion"
              role="tabpanel"
            >
              <b-card-body>
                <b-card-text class="accordion-text"
                  >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam
                  dignissim laoreet lobortis. Duis scelerisque, quam id ornare
                  tempus, nunc lorem blandit quam, vel euismod ligula tortor id
                  mi. Vestibulum vehicula placerat condimentum.</b-card-text
                >
              </b-card-body>
            </b-collapse>
          </b-card>
          <b-card no-body class="mb-1 accordion">
            <b-card-header
              header-tag="header"
              class="accordion-header"
              role="tab"
              v-b-toggle.accordion-8
            >
              <h6>8. Applicable law and place of jurisdiction</h6>
              <span class="when-open h4 mb-2"
                ><b-icon-dash class="accordion-dash-icon"></b-icon-dash
              ></span>
              <span class="when-closed h4 mb-2"
                ><b-icon-plus></b-icon-plus
              ></span>
            </b-card-header>
            <b-collapse
              id="accordion-8"
              visible
              accordion="my-accordion"
              role="tabpanel"
            >
              <b-card-body>
                <b-card-text class="accordion-text"
                  >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam
                  dignissim laoreet lobortis. Duis scelerisque, quam id ornare
                  tempus, nunc lorem blandit quam, vel euismod ligula tortor id
                  mi. Vestibulum vehicula placerat condimentum.</b-card-text
                >
              </b-card-body>
            </b-collapse>
          </b-card>
        </div>
      </div>
    </div>
    <footer>
      <div class="container">
        <div class="footer-block">
          <div class="imageContent">
            <img width="191px" height="40px" src="@/assets/logo.png" />
            <div class="social-media">
              <img class="social social-footer" src="@/assets/Facebook.svg" />
              <img class="social social-footer" src="@/assets/Instagram.svg" />
            </div>
            <div class="footer-text">
              <h5 class="text">Inventory</h5>
              <h5 class="text">Financing</h5>
              <h5 class="text">Contacts</h5>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      email: "",
      emailValidationStatus: "",
      filterStatus: {
        yearsFilterState: "Year",
        carFilterState: "Car",
        modelFilterState: "Model",
        trimFilterState: "Trim",
        mileageFilterState: "Mileage",
      },
      yearsFilterOptions: [2014, 2015, 2016, 2017, 2019, "Year"],
      carFilterOptions: [
        "Audi",
        "BMW",
        "Mercedes",
        "Toyota",
        "Volkswagen",
        "Car",
      ],
      modelFilterOptions: ["A4", "X5", "S-Class", "Camry", "Tiguan", "Model"],
      trimFilterOptions: ["New", "Trim"],
      mileageFilterOptions: [
        "19,855 km",
        "39,850 km",
        "69,000 km",
        "86,000 km",
        "104,000 km",
        "Mileage",
      ],
      filteredCarsCounter: 0,
      sortSelectState: "Publication date (descending)",
      sortOptions: [
        "Publication date (descending)",
        "Publication date (ascending)",
        "Price (descending)",
        "Price (ascending)",
        "Mileage (descending)",
        "Mileage (ascending)",
      ],
      carsTestData: [],
      fullCarsTestData: [
        {
          year: 2019,
          model: "Volkswagen Tiguan",
          trim: "New",
          mileage: "19,855 km",
          price: "$34,000",
        },
        {
          year: 2017,
          model: "Audi A4",
          trim: "Old",
          mileage: "25,855 km",
          price: "$54,000",
        },
        {
          year: 2019,
          model: "BMW X5",
          trim: "New",
          mileage: "39,850 km",
          price: "$74,000",
        },
        {
          year: 2019,
          model: "Mercedes S-Class",
          trim: "New",
          mileage: "69,000 km",
          price: "$99,000",
        },
        {
          year: 2015,
          model: "Toyota Camry",
          trim: "Old",
          mileage: "86,000 km",
          price: "$124,000",
        },
        {
          year: 2014,
          model: "Audi A4",
          trim: "New",
          mileage: "104,000 km",
          price: "$149,000",
        },
        {
          year: 2019,
          model: "BMW X5",
          trim: "Old",
          mileage: "19,855 km",
          price: "$34,000",
        },
        {
          year: 2019,
          model: "Audi A4",
          trim: "New",
          mileage: "25,855 km",
          price: "$54,000",
        },
        {
          year: 2019,
          model: "BMW X5",
          trim: "New",
          mileage: "39,850 km",
          price: "$74,000",
        },
        {
          year: 2019,
          model: "Mercedes S-Class",
          trim: "Old",
          mileage: "69,000 km",
          price: "$99,000",
        },
        {
          year: 2016,
          model: "Toyota Camry",
          trim: "New",
          mileage: "86,000 km",
          price: "$124,000",
        },
        {
          year: 2014,
          model: "Audi A4",
          trim: "Old",
          mileage: "104,000 km",
          price: "$149,000",
        },
        {
          year: 2019,
          model: "BMW X5",
          trim: "New",
          mileage: "19,855 km",
          price: "$34,000",
        },
        {
          year: 2019,
          model: "Audi A4",
          trim: "New",
          mileage: "25,855 km",
          price: "$54,000",
        },
        {
          year: 2016,
          model: "BMW X5",
          trim: "New",
          mileage: "39,850 km",
          price: "$74,000",
        },
        {
          year: 2016,
          model: "Mercedes S-Class",
          trim: "Old",
          mileage: "69,000 km",
          price: "$99,000",
        },
        {
          year: 2015,
          model: "Toyota Camry",
          trim: "New",
          mileage: "86,000 km",
          price: "$124,000",
        },
        {
          year: 2014,
          model: "Audi A4",
          trim: "Old",
          mileage: "104,000 km",
          price: "$149,000",
        },
        {
          year: 2017,
          model: "BMW X5",
          trim: "New",
          mileage: "19,855 km",
          price: "$34,000",
        },
        {
          year: 2014,
          model: "Mercedes S-Class",
          trim: "New",
          mileage: "69,000 km",
          price: "$99,000",
        },
        {
          year: 2015,
          model: "Toyota Camry",
          trim: "Old",
          mileage: "86,000 km",
          price: "$124,000",
        },
        {
          year: 2014,
          model: "Audi A4",
          trim: "New",
          mileage: "104,000 km",
          price: "$149,000",
        },
      ],
    };
  },
  created() {
    this.carsTestData = this.fullCarsTestData;
    this.filteredCarsCounter = this.fullCarsTestData.length;
  },
  methods: {
    filterByYear(year) {
      this.filterStatus.yearsFilterState = `${year}`;
    },
    filterByCar(car) {
      this.filterStatus.carFilterState = car;
    },
    filterByModel(model) {
      this.filterStatus.modelFilterState = model;
    },
    filterByTrim(trim) {
      this.filterStatus.trimFilterState = trim;
    },
    filterByMileage(mileage) {
      this.filterStatus.mileageFilterState = mileage;
    },
    sortBy(sort) {
      this.sortSelectState = sort;
    },
    sendEmail() {
      let emailRegex = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/;
      this.emailValidationStatus = emailRegex.test(this.email)
        ? "email-success"
        : "email-error";
    },
  },
  watch: {
    email() {
      this.emailValidationStatus = "";
    },
    filterStatus: {
      handler(newFilterState) {
        this.carsTestData = this.fullCarsTestData.filter((car) => {
          return (
            (newFilterState.yearsFilterState !== "Year"
              ? car.year === parseInt(newFilterState.yearsFilterState)
              : true) &&
            (newFilterState.carFilterState !== "Car"
              ? car.model.split(" ")[0] === newFilterState.carFilterState
              : true) &&
            (newFilterState.modelFilterState !== "Model"
              ? car.model.split(" ")[1] === newFilterState.modelFilterState
              : true) &&
            (newFilterState.trimFilterState !== "Trim"
              ? car.trim === newFilterState.trimFilterState
              : true) &&
            (newFilterState.mileageFilterState !== "Mileage"
              ? car.mileage === newFilterState.mileageFilterState
              : true)
          );
        });
        this.filteredCarsCounter = this.carsTestData.length;
      },
      deep: true,
    },
    sortSelectState: {
      handler(newSortState) {
        this.carsTestData = this.carsTestData.sort((a, b) => {
          if (newSortState === "Publication date (descending)") {
            return a.year > b.year ? -1 : 1;
          } else if (newSortState === "Publication date (ascending)") {
            return a.year < b.year ? -1 : 1;
          } else if (newSortState === "Price (descending)") {
            return a.price > b.price ? -1 : 1;
          } else if (newSortState === "Price (ascending)") {
            return a.price < b.price ? -1 : 1;
          } else if (newSortState === "Mileage (descending)") {
            return a.mileage > b.mileage ? -1 : 1;
          } else if (newSortState === "Mileage (ascending)") {
            return a.mileage < b.mileage ? -1 : 1;
          }
        });
      },
    },
  },
  computed: {
    isAscending() {
      return this.sortSelectState.includes("ascending");
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans:wght@100;200;300;400;500;600;700;800;900&display=swap");
$background-color: #f8f8f8;

#app html,
body,
ul,
a {
  font-family: "Noto Sans", sans !important;
  background-color: $background-color !important;

  &:focus,
  &:active {
    background-color: $background-color !important;
    color: black !important;
  }
}
.header {
  background-image: url("@/assets/background-img.png");
  background-size: 100%;
  height: 688px;
}
.container-header {
  display: flex;
  justify-content: space-between;
}
.header-navbar {
  display: flex;
  align-items: center;
  justify-content: center;
}
.points {
  color: white;
  padding: 0 40px;
  margin-bottom: 0rem;
}
.social {
  padding-left: 30px;
}
p {
  color: white;
  font-size: 15px;
  font-weight: 400;
}
.firstTitle {
  color: #aeaeae;
  font-size: 50px;
}
.descr-container {
  display: flex;
  gap: 45px;
  margin-top: 32px;
}
.descr-container__descr-header {
  width: 295px;
}
.container-2 {
  margin-top: 136px;
}
.button {
  border: 2px solid white;
  border-radius: 15px;
  padding: 0px 90px;
  height: 56px;
  color: white;
  font-size: 20px;
  margin-top: 72px;
  background: transparent;

  &:hover {
    border: 2px solid $background-color;
    color: black;
    background: $background-color;
  }

  &:active {
    border: 2px solid white;
    color: white;
    background: transparent;
  }
}
.btn-cars {
  margin-top: 0 !important;
  border: none;
}
.logo {
  margin-top: 19px;
}
.check {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-top: 25px !important;
  padding-right: 35px;
  align-items: baseline;
}
.sort {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin-right: 3rem;
}
.sort-li {
  border: none !important;
  position: relative;
  bottom: 2.5px;
  right: 17px;
}
.drop-sort::after {
  display: none !important;
}
.img-sort {
  position: relative;
  bottom: 2px;
}
h6 {
  font-size: 12px;
}
.nav-item {
  border: 1px solid #bdbdbd !important;
  border-radius: 15px !important;
  margin-right: 20px !important;
  padding: 3px 0;
  width: 170px;
  height: 48px;

  > button {
    border: none !important;
    background: transparent !important;
    color: #212529 !important;
    display: flex;
    align-items: center;
    justify-content: space-between;

    &:focus {
      box-shadow: none !important;
    }
  }
}
.selected-filter-option {
  > a,
  a:hover {
    color: #df4e3d;
  }
}
.filter-selected-state {
  border-color: #df4e3d !important;
}
.filter-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 30px;
}
.sort-block {
  border: none !important;
  max-width: 100%;

  button {
    color: #8d8d8d !important;
    align-items: normal !important;
  }

  > button::after {
    display: none !important;
  }
}
.nav-link {
  color: black !important;
  display: flex !important;
  justify-content: space-between;
  align-items: center;
  font-size: 15px;
}
.btn-cars {
  background: #df4e3c;
  color: white;
  padding: 0 120px;
  font-size: 15px;
  height: 48px;
}
.btn-cars:active:hover {
  background: #df4e3c;
  color: white;
  padding: 0 120px;
  font-size: 15px;
}
.btn-cars:hover {
  background: #df4e3c;
  color: white;
  padding: 0 120px;
  font-size: 15px;
}
.btn-cars:focus {
  background: #df4e3c;
  color: white;
  padding: 0 120px;
  font-size: 15px;
}
.btn-cars:disabled {
  background: #df4e3c;
  color: white;
  padding: 0 120px;
  font-size: 15px;
  opacity: 0.5;
}
.form-check-input:checked[type="checkbox"] {
  background-color: #df4e3c;
  border: none;
  box-shadow: none;
}
.form-check-input:focus {
  border: none;
  box-shadow: none;
  /* background: $background-color; */
  border: 1px solid #8d8d8d;
}
.cars-block {
  margin-top: 100px;
}
.navbar {
  margin-top: 80px;
}
.car {
  height: auto;
  background: white;
  box-shadow: 0px 0px 18px rgba(68, 68, 68, 0.09);
  padding: 0 0 35px 0 !important;
  max-width: 450px !important;
}
.car:hover {
  opacity: 0.9;
}
.year {
  color: #bdbdbd;
}
.model {
  color: black;
  font-size: 17px;
}
.aboutCar {
  display: flex;
  align-items: baseline;
  font-size: 17px;
  gap: 8px;
}
.carInfo {
  display: flex;
  padding: 16px 0 27px 0;
  font-size: 14px;
}
.info {
  color: #8d8d8d;
}
.car-descr {
  padding-left: 20px;
}
.vr {
  display: inline;
  height: 8px;
  margin: 4px 5px 0;
  border-left: 1.5px solid #8d8d8d;
}
.car {
  /* margin-left: 20px; */
}
.form-check-input {
  background: $background-color;
}
.cars-container {
  --bs-gutter-x: 0;
  column-gap: 27px;
  row-gap: 20px;
}
.car-descr {
  margin-top: 20px;
}
.sold {
  opacity: 0.5;
}
.form-control {
  width: 505px !important;
  height: 54px;
  border-radius: 15px !important;
  margin-right: 20px;
  background: black !important;
  color: white !important;
  border: 1px solid #bdbdbd;
}
.form-control:focus {
  box-shadow: none !important;
  background-color: black;
  color: white;
  border: 1px solid white !important;
}
.subscribe {
  margin-bottom: 10px;
}
.subsc-btn {
  margin-top: 0;
  padding: 0 30px;
  background: $background-color;
  color: black;
  font-size: 15px;
  font-weight: 400;

  &:hover:enabled {
    background: black;
    border: 2px solid white;
    color: white;
  }

  &:active:enabled {
    border: 2px solid white;
    color: black;
    background: $background-color;
  }

  &:disabled {
    opacity: 0.8;
    cursor: not-allowed;
  }
}
.question-block {
  background-color: black;
  align-items: center;
  padding: 40px 0;
  margin-top: 120px;
}
.subscribe-text {
  display: flex;
  flex-direction: column;
  padding-right: 95px;
}
.subs-descr {
  width: 429px;
  margin-top: 12px;
  font-size: 15px;
}
.subs-title {
  color: white;
}
::placeholder {
  color: white !important;
  font-size: 15px;
}
.accordion-block {
  --bs-gutter-x: 0;
}
body {
  background-color: $background-color;
}
.mb-80 {
  margin-bottom: 80px;
  margin-top: 120px;
  font-weight: 700;
}
.accordion-button:not(.collapsed) {
  background: white !important;
  color: #df4e3c !important;
  border: none !important;
  padding: 24px 46px 0 39px;
  font-size: 15px;
  font-weight: 700;
}
.accordion-body {
  box-shadow: 0px 0px 18px rgb(68 68 68 / 9%);
}
.accordion {
  border: none !important;
  cursor: pointer;

  &-text {
    color: black;
    font-size: 13px;
    padding-left: 1.5rem;
  }

  &-header {
    border: none !important;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 24px !important;
  }
}

.collapsed > .when-open,
.not-collapsed > .when-closed {
  display: none;
}
.collapsed {
  background-color: $background-color !important;
  border-bottom: 2px solid #c4c4c4 !important;
  font-size: 15px;
  font-weight: 500;
}
.not-collapsed {
  background-color: white !important;
  border: none !important;
  font-size: 15px;
  font-weight: 500;
  padding: 24px 24px 0 40px !important;
  color: #df4e3c !important;
}
.accordion-button {
  background: linear-gradient(0deg, #c4c4c4 1%, $background-color 0%);
  margin-top: 10px;
}
.accordion-item:active {
  box-shadow: 0px 0px 18px rgba(68, 68, 68, 0.09);
}
footer {
  background: black;
  margin-top: 200px;
}
.imageContent {
  padding: 37px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.social-footer {
  padding: 15px;
}
.footer-text {
  display: flex;
  order: 1;
  gap: 40px;
}
.social-media {
  display: flex;
  order: 2;
}
.text {
  color: white;
  font-size: 15px;
  font-weight: 600;
  margin: 0;
}
.carImage {
  height: auto;
  width: 100%;
  width: -moz-available;
  width: -webkit-fill-available;
  width: fill-available;
}
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, auto));
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
.email-success {
  border: 1px solid #507a36 !important;
}
.email-error {
  border: 1px solid #df4e3c !important;
}
.errorMessage {
  color: #df4e3c !important;
  font-size: 10px;
  font-weight: 400;
  position: absolute;
  left: 20px;
  top: 60px;
}
.subscribe-input {
  position: relative;
}
.input-icon {
  position: absolute;
  right: 40px;
  top: 50%;
  transform: translateY(-50%);
}
.success-input-icon {
  color: #507a36 !important;
}
.error-input-icon {
  color: #df4e3c !important;
}
.ascending {
  -webkit-transform: rotate(180deg);
  -moz-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  -o-transform: rotate(180deg);
}
.accordion-dash-icon {
  color: #df4e3c !important;
  margin-right: 10px;
}
</style>
