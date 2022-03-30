<template>
  <div>
    <p v-if="queryLoading">Query is loading</p>
    <p v-else-if="queryError">Query failed {{ queryError }}</p>
    <p v-else-if="queryResult">Query succeeded {{ queryResult }}</p>
  </div>

  <div>
    <button  @click="tryMutation">Try mutation</button>
    <p v-if="mutationLoading">Mutation is loading</p>
    <p v-else-if="mutationError">Mutation failed {{ mutationError }}</p>
    <p v-else-if="mutationResult">Mutation succeeded {{ mutationResult }}</p>
  </div>
</template>

<script setup>
import { useQuery, useMutation } from "@vue/apollo-composable";
import gql from "graphql-tag";

const {
  result: queryResult,
  loading: queryLoading,
  error: queryError,
} = useQuery(gql`
  query {
    post(id: 1) {
      id
      title
      body
    }
  }
`);

const mutation = gql`
  mutation ($id: ID!, $input: UpdatePostInput!) {
    updatePost(id: $id, input: $input) {
      id
      body
    }
  }
`;

const {
  loading: mutationLoading,
  result: mutationResult,
  error: mutationError,
  mutate,
} = useMutation(mutation);

const tryMutation = () => {
  mutate({
    id: 1,
    input: {
      body: "Some updated content.",
    },
  });
};
</script>

<style>
</style>
