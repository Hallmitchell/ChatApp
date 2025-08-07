<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import { currentUser, pb } from './pocketbase';
    let newMessage: string;
    let messages: any[] = [];