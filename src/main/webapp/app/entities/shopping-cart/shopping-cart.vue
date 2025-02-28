<template>
  <div>
    <h2 id="page-heading" data-cy="ShoppingCartHeading">
      <span v-text="t$('boutiqueApp.shoppingCart.home.title')" id="shopping-cart-heading"></span>
      <div class="d-flex justify-content-end">
        <button class="btn btn-info mr-2" v-on:click="handleSyncList" :disabled="isFetching">
          <font-awesome-icon icon="sync" :spin="isFetching"></font-awesome-icon>
          <span v-text="t$('boutiqueApp.shoppingCart.home.refreshListLabel')"></span>
        </button>
        <router-link :to="{ name: 'ShoppingCartCreate' }" custom v-slot="{ navigate }">
          <button
            @click="navigate"
            id="jh-create-entity"
            data-cy="entityCreateButton"
            class="btn btn-primary jh-create-entity create-shopping-cart"
          >
            <font-awesome-icon icon="plus"></font-awesome-icon>
            <span v-text="t$('boutiqueApp.shoppingCart.home.createLabel')"></span>
          </button>
        </router-link>
      </div>
    </h2>
    <br />
    <div class="alert alert-warning" v-if="!isFetching && shoppingCarts && shoppingCarts.length === 0">
      <span v-text="t$('boutiqueApp.shoppingCart.home.notFound')"></span>
    </div>
    <div class="table-responsive" v-if="shoppingCarts && shoppingCarts.length > 0">
      <table class="table table-striped" aria-describedby="shoppingCarts">
        <thead>
          <tr>
            <th scope="row"><span v-text="t$('global.field.id')"></span></th>
            <th scope="row"><span v-text="t$('boutiqueApp.shoppingCart.placedDate')"></span></th>
            <th scope="row"><span v-text="t$('boutiqueApp.shoppingCart.status')"></span></th>
            <th scope="row"><span v-text="t$('boutiqueApp.shoppingCart.totalPrice')"></span></th>
            <th scope="row"><span v-text="t$('boutiqueApp.shoppingCart.paymentMethod')"></span></th>
            <th scope="row"><span v-text="t$('boutiqueApp.shoppingCart.paymentReference')"></span></th>
            <th scope="row"><span v-text="t$('boutiqueApp.shoppingCart.customerDetails')"></span></th>
            <th scope="row"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="shoppingCart in shoppingCarts" :key="shoppingCart.id" data-cy="entityTable">
            <td>
              <router-link :to="{ name: 'ShoppingCartView', params: { shoppingCartId: shoppingCart.id } }">{{
                shoppingCart.id
              }}</router-link>
            </td>
            <td>{{ formatDateShort(shoppingCart.placedDate) || '' }}</td>
            <td v-text="t$('boutiqueApp.OrderStatus.' + shoppingCart.status)"></td>
            <td>{{ shoppingCart.totalPrice }}</td>
            <td v-text="t$('boutiqueApp.PaymentMethod.' + shoppingCart.paymentMethod)"></td>
            <td>{{ shoppingCart.paymentReference }}</td>
            <td>
              <div v-if="shoppingCart.customerDetails">
                <router-link :to="{ name: 'CustomerDetailsView', params: { customerDetailsId: shoppingCart.customerDetails.id } }">{{
                  shoppingCart.customerDetails.id
                }}</router-link>
              </div>
            </td>
            <td class="text-right">
              <div class="btn-group">
                <router-link :to="{ name: 'ShoppingCartView', params: { shoppingCartId: shoppingCart.id } }" custom v-slot="{ navigate }">
                  <button @click="navigate" class="btn btn-info btn-sm details" data-cy="entityDetailsButton">
                    <font-awesome-icon icon="eye"></font-awesome-icon>
                    <span class="d-none d-md-inline" v-text="t$('entity.action.view')"></span>
                  </button>
                </router-link>
                <router-link :to="{ name: 'ShoppingCartEdit', params: { shoppingCartId: shoppingCart.id } }" custom v-slot="{ navigate }">
                  <button @click="navigate" class="btn btn-primary btn-sm edit" data-cy="entityEditButton">
                    <font-awesome-icon icon="pencil-alt"></font-awesome-icon>
                    <span class="d-none d-md-inline" v-text="t$('entity.action.edit')"></span>
                  </button>
                </router-link>
                <b-button
                  v-on:click="prepareRemove(shoppingCart)"
                  variant="danger"
                  class="btn btn-sm"
                  data-cy="entityDeleteButton"
                  v-b-modal.removeEntity
                >
                  <font-awesome-icon icon="times"></font-awesome-icon>
                  <span class="d-none d-md-inline" v-text="t$('entity.action.delete')"></span>
                </b-button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <b-modal ref="removeEntity" id="removeEntity">
      <template #modal-title>
        <span
          id="boutiqueApp.shoppingCart.delete.question"
          data-cy="shoppingCartDeleteDialogHeading"
          v-text="t$('entity.delete.title')"
        ></span>
      </template>
      <div class="modal-body">
        <p id="jhi-delete-shoppingCart-heading" v-text="t$('boutiqueApp.shoppingCart.delete.question', { id: removeId })"></p>
      </div>
      <template #modal-footer>
        <div>
          <button type="button" class="btn btn-secondary" v-text="t$('entity.action.cancel')" v-on:click="closeDialog()"></button>
          <button
            type="button"
            class="btn btn-primary"
            id="jhi-confirm-delete-shoppingCart"
            data-cy="entityConfirmDeleteButton"
            v-text="t$('entity.action.delete')"
            v-on:click="removeShoppingCart()"
          ></button>
        </div>
      </template>
    </b-modal>
  </div>
</template>

<script lang="ts" src="./shopping-cart.component.ts"></script>
