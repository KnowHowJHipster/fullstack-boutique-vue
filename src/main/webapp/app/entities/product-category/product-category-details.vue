<template>
  <div class="row justify-content-center">
    <div class="col-8">
      <div v-if="productCategory">
        <h2 class="jh-entity-heading" data-cy="productCategoryDetailsHeading">
          <span v-text="t$('boutiqueApp.productCategory.detail.title')"></span> {{ productCategory.id }}
        </h2>
        <dl class="row jh-entity-details">
          <dt>
            <span v-text="t$('boutiqueApp.productCategory.name')"></span>
          </dt>
          <dd>
            <span>{{ productCategory.name }}</span>
          </dd>
          <dt>
            <span v-text="t$('boutiqueApp.productCategory.description')"></span>
          </dt>
          <dd>
            <span>{{ productCategory.description }}</span>
          </dd>
        </dl>
        <button type="submit" v-on:click.prevent="previousState()" class="btn btn-info" data-cy="entityDetailsBackButton">
          <font-awesome-icon icon="arrow-left"></font-awesome-icon>&nbsp;<span v-text="t$('entity.action.back')"></span>
        </button>
        <router-link
          v-if="productCategory.id"
          :to="{ name: 'ProductCategoryEdit', params: { productCategoryId: productCategory.id } }"
          custom
          v-slot="{ navigate }"
        >
          <button @click="navigate" class="btn btn-primary">
            <font-awesome-icon icon="pencil-alt"></font-awesome-icon>&nbsp;<span v-text="t$('entity.action.edit')"></span>
          </button>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script lang="ts" src="./product-category-details.component.ts"></script>
