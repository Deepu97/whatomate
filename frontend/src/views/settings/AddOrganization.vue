<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card'
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { usersService, organizationService } from '@/services/api';
import { Settings, Bell, Loader2 } from 'lucide-vue-next';
import { toast } from 'vue-sonner'
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'
import { Separator } from '@/components/ui/separator'

const isSubmitting = ref(false);
const isLoading = ref(true);

const generalSettings = ref({
  organization_name: '',
  default_timezone: 'UTC',
  date_format: 'YYYY-MM-DD',
  mask_phone_numbers: false
})

async function saveGeneralSettings() {
  isSubmitting.value = true
  try {
    await organizationService.insertOrg({
      name: generalSettings.value.organization_name,
      timezone: generalSettings.value.default_timezone,
      date_format: generalSettings.value.date_format,
      mask_phone_numbers: generalSettings.value.mask_phone_numbers, // ✅ add this
    })
    toast.success('Organization created')
  } catch (error) {
    toast.error('Failed to create organization')
  } finally {
    isSubmitting.value = false
  }
}

</script>
<template>
   <header class="border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60">
      <div class="flex h-16 items-center px-6">
       
        <div class="flex-1">
          <h1 class="text-xl font-semibold">Add Organization</h1>
          <p class="text-sm text-muted-foreground">Add multiple organization </p>
        </div>
      </div>
    </header>
     

    <Card>
              <CardHeader>
                <CardTitle>Add Organization</CardTitle>
                <CardDescription>Add organization</CardDescription>
              </CardHeader>
              <CardContent class="space-y-4">
                <div class="space-y-2">
                  <Label for="org_name">Organization Name</Label>
                  <Input
                    id="org_name"
                    v-model="generalSettings.organization_name"
                    placeholder="Add your organization"
                  />
                </div>
                <div class="grid grid-cols-2 gap-4">
                  <div class="space-y-2">
                    <Label for="timezone">Default Timezone</Label>
                    <Select v-model="generalSettings.default_timezone">
                      <SelectTrigger>
                        <SelectValue placeholder="Select timezone" />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectItem value="UTC">UTC</SelectItem>
                        <SelectItem value="America/New_York">Eastern Time</SelectItem>
                        <SelectItem value="America/Los_Angeles">Pacific Time</SelectItem>
                        <SelectItem value="Europe/London">London</SelectItem>
                        <SelectItem value="Asia/Tokyo">Tokyo</SelectItem>
                      </SelectContent>
                    </Select>
                  </div>
                  <div class="space-y-2">
                    <Label for="date_format">Date Format</Label>
                    <Select v-model="generalSettings.date_format">
                      <SelectTrigger>
                        <SelectValue placeholder="Select format" />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectItem value="YYYY-MM-DD">YYYY-MM-DD</SelectItem>
                        <SelectItem value="DD/MM/YYYY">DD/MM/YYYY</SelectItem>
                        <SelectItem value="MM/DD/YYYY">MM/DD/YYYY</SelectItem>
                      </SelectContent>
                    </Select>
                  </div>
                </div>
                <Separator />
                <!-- <div class="flex items-center justify-between">
                  <div>
                    <p class="font-medium">Mask Phone Numbers</p>
                    <p class="text-sm text-muted-foreground">Hide phone numbers showing only last 4 digits</p>
                  </div>
                  <Switch
                    :checked="generalSettings.mask_phone_numbers"
                    @update:checked="generalSettings.mask_phone_numbers = $event"
                  />
                </div> -->
                <div class="flex justify-end">
                  <Button variant="outline" size="sm" @click="saveGeneralSettings" :disabled="isSubmitting">
                    <Loader2 v-if="isSubmitting" class="mr-2 h-4 w-4 animate-spin" />
                    Add organization
                  </Button>
                </div>
              </CardContent>
            </Card>
     

</template>
