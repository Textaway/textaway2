"use client";

import React, { useState, useRef } from "react";
import { User } from "@/entities/User";
import { UploadFile } from "@/integrations/Core";
import { useToast } from "@/components/ui/use-toast";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Textarea } from "@/components/ui/textarea";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { Upload, Loader2 } from "lucide-react";

interface ProfileFormProps {
  currentUser: any;
}

export default function ProfileForm({ currentUser }: ProfileFormProps) {
  const { toast } = useToast();
  const [formData, setFormData] = useState({
    firstName: currentUser.firstName || "",
    lastName: currentUser.lastName || "",
    bio: currentUser.bio || "",
    website: currentUser.website || "",
    profileImageUrl: currentUser.profileImageUrl || "",
    socialLinks: {
      twitter: currentUser.socialLinks?.twitter || "",
      github: currentUser.socialLinks?.github || "",
      linkedin: currentUser.socialLinks?.linkedin || "",
    },
  });
  const [loading, setLoading] = useState(false);
  const [uploading, setUploading] = useState(false);
  const fileInputRef = useRef<HTMLInputElement>(null);

  const handleFileChange = async (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (!file) return;

    setUploading(true);
    try {
      const response = await UploadFile({ file });
      setFormData({ ...formData, profileImageUrl: response.file_url });
      toast({ title: "Success", description: "Profile image updated." });
    } catch (error) {
      console.error("Image upload failed:", error);
      toast({ title: "Error", description: "Failed to upload image.", variant: "destructive" });
    } finally {
      setUploading(false);
    }
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);
    try {
      await User.updateMyUserData(formData);
      toast({ title: "Success", description: "Profile updated successfully." });
    } catch (error) {
      console.error("Profile update failed:", error);
      toast({ title: "Error", description: "Failed to update profile.", variant: "destructive" });
    } finally {
      setLoading(false);
    }
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-6 bg-neutral-900 border border-neutral-800 rounded-lg p-6">
      <div className="flex items-center gap-4">
        <Avatar className="h-20 w-20 border-2 border-neutral-800">
          <AvatarImage src={formData.profileImageUrl} alt="Profile" />
          <AvatarFallback className="bg-neutral-800 text-neutral-50 text-2xl">
            {formData.firstName?.[0] || currentUser.email[0].toUpperCase()}
          </AvatarFallback>
        </Avatar>
        <div>
          <Button type="button" onClick={() => fileInputRef.current?.click()} disabled={uploading}>
            {uploading ? (
              <Loader2 className="h-4 w-4 mr-2 animate-spin" />
            ) : (
              <Upload className="h-4 w-4 mr-2" />
            )}
            Upload Image
          </Button>
          <input type="file" ref={fileInputRef} onChange={handleFileChange} className="hidden" accept="image/*" />
          <p className="text-xs text-neutral-500 mt-2">PNG, JPG, GIF up to 10MB</p>
        </div>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div className="space-y-2">
          <Label htmlFor="firstName">First Name</Label>
          <Input id="firstName" value={formData.firstName} onChange={(e) => setFormData({ ...formData, firstName: e.target.value })} className="bg-neutral-950 border-neutral-800" />
        </div>
        <div className="space-y-2">
          <Label htmlFor="lastName">Last Name</Label>
          <Input id="lastName" value={formData.lastName} onChange={(e) => setFormData({ ...formData, lastName: e.target.value })} className="bg-neutral-950 border-neutral-800" />
        </div>
      </div>

      <div className="space-y-2">
        <Label htmlFor="bio">Bio</Label>
        <Textarea id="bio" value={formData.bio} onChange={(e) => setFormData({ ...formData, bio: e.target.value })} rows={3} className="bg-neutral-950 border-neutral-800" />
      </div>

      <div className="space-y-2">
        <Label htmlFor="website">Website</Label>
        <Input id="website" type="url" value={formData.website} onChange={(e) => setFormData({ ...formData, website: e.target.value })} className="bg-neutral-950 border-neutral-800" />
      </div>

      <div className="space-y-4">
        <Label>Social Links</Label>
        <div className="space-y-2">
          <Input placeholder="Twitter URL" value={formData.socialLinks.twitter} onChange={(e) => setFormData({ ...formData, socialLinks: { ...formData.socialLinks, twitter: e.target.value } })} className="bg-neutral-950 border-neutral-800" />
          <Input placeholder="GitHub URL" value={formData.socialLinks.github} onChange={(e) => setFormData({ ...formData, socialLinks: { ...formData.socialLinks, github: e.target.value } })} className="bg-neutral-950 border-neutral-800" />
          <Input placeholder="LinkedIn URL" value={formData.socialLinks.linkedin} onChange={(e) => setFormData({ ...formData, socialLinks: { ...formData.socialLinks, linkedin: e.target.value } })} className="bg-neutral-950 border-neutral-800" />
        </div>
      </div>

      <div className="flex justify-end">
        <Button type="submit" disabled={loading} className="bg-neutral-50 text-neutral-950 hover:bg-neutral-200">
          {loading && <Loader2 className="h-4 w-4 mr-2 animate-spin" />}
          Save Changes
        </Button>
      </div>
    </form>
  );
}
